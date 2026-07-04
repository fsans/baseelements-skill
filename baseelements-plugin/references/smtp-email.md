# SMTP Email — BaseElements Plugin

## BE_SMTPServer

```
BE_SMTPServer ( server ; { port ; username ; password ; keepOpen } )
```

**Description**

Stores the SMTP connection details for future calls to the *BE_SMTPSend* function.

**Parameters**

* *server* : A domain name or IP address of the SMTP server to connect to.
* *port* : the port number ( a required parameter, but can be an empty string ).
* *username* ( optional ) : The username if the server requires authentication.
* *password* ( optional ) : The password if the server requires authentication.
* *keepOpen* ( optional, default:False , ProOnly ) : Whether to keep the connection open for future sends.

To use the *keepOpen* option, set this to True when sending multiple messages via SMTP in a loop to avoid closing the connection to the server and re-opening it every time.  This will be faster and less work for the server and the plugin.  To close the connection, use the same command but change the keepOpen parameter to False.

**Version History**

* 3.1.0 : First Release
* 4.0.2 : Renamed from BE_SMTP_Server

**Notes**

This function doesn't connect to the server itself it only stores the connection details for future *BE_SMTPSend* function calls.

Authentication is controlled with curl, so additional options, other than the defaults can be set with the *BE_CurlSetOption* function, and headers can be set via the *BE_SMTPSetHeader* function.

Any future calls to this function overwrite any existing details.

port number can be left as an empty string, which will attempt to use whatever ports the server requires, default for SMTP is 25, but using SSL or TLS may use 465 or 587.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

---

## BE_SMTPSend

```
BE_SMTPSend ( from ; to ; subject ; text ; { cc ; bcc ; replyTo ; html ; attachments} )
```

**Description**

Sends an email via SMTP. Use in conjunction with a call to BE_SMTPServer prior to calling this function. The To, CC, BCC and attachments parameteres can be passed value lists in order to send to multiple people or to include multiple attachments.

**Parameters**

* *from* : the from name and email address.
* *to* : the to name and email address.
* *subject* : subject line for the email ( a required parameter, but can be an empty string ).
* *text* : text content of the email ( a required parameter, but can be an empty string ).
* *cc* ( optional ) : the cc name and email address.
* *bcc* ( optional ) : the bcc name and email address.
* *replyTo* ( optional ) : the replyTo name and email address.
* *html* ( optional ) : the text of the HTML version of the email content.
* *attachments* ( optional ) : a list of file paths for attachments to be included.

**Version History**

* 3.1.0 : First Release
* 4.0.0 : Added an RFC 1123 format date header
* 4.0.2 : Renamed from BE_SMTPSend

**Notes**

The content of emails on some servers appears to strip FileMaker line endings. Try replacing Char ( 13 ) with Char ( 10 ) in the email content.

Some servers, in particular various Exchange or Office365 based servers use a different authentication method. When attempting SMTP normally you may get a 35 error. You can force the correct method with :

	BE_CurlSetOption ( "BE_CURLOPT_FORCE_STARTTLS" ; True )

before doing BE_SMTPSend.

**About HTML Content**

HTML content is complex and not easily setup.  We recommend you use an external process or application to generate the HTML content, and use either embedded base64 images, or links to externally hosted images.  There is not support for multipart mime messages that would let you attach multiple images and then use them inline inside the HTML content.

There are lots of examples and help around crafting HTML email content such as :

[https://sendgrid.com/en-us/blog/create-html-emails](https://sendgrid.com/en-us/blog/create-html-emails) however we make no guarantees around the ability of the plugin to send your HTML email and have it arrive successfully and look as intended.

We recommend people not use SMTP for email and use the various services that have APIs instead.

**About SMTP Send and "Sent" Folders**

A message being put into sent folder is a function of the mail client that sends it : it creates the message to send, sends a copy via the SMTP server, and then on success gives the IMAP server a second copy to save into the users *Sent* folder via IMAP.

The BE plugin SMTP only does the SMTP send part, so in order to save a copy in a Sent folder, you'd need to have use IMAP functions to save to the sent folder, along with the correct IMAP server credentials, which may be different than the SMTP server and credentials.  This is technically possible using the HTTP functions and curl options, but is beyond the scope of the intended use of this function.

Other options for retaining sent emails are :

* Some email services let you use an API instead of SMTP, ( eg google or O365 ) and that API may keep copies in the sent folder.
* BCC to an archive email address that stores old copies on the mail server.
* Keep them in a table in FileMaker instead.

See the *BE_SMTPAddAttachment* documentation for sending attachments from container fields.

---

Authentication is controlled with curl, so additional options, other than the defaults can be set with the *BE_CurlSetOption* function, and headers can be set via the *BE_SMTPSetHeader* function.

Not all servers respond with data when doing curl operations so use *BE_GetLastError* after the function call to tell if the call was able to be made.

Error codes that you get from the *BE_GetLastError* function after this function call comes from the curl library itself and not the plugin.  To find a specific error code use this documentation :

[http://curl.haxx.se/libcurl/c/libcurl-errors.html](http://curl.haxx.se/libcurl/c/libcurl-errors.html)

Also use the *BE_CurlTrace* function to see the transcript of the connection to diagnose any other issues that may have come up and can't be determined from the error value alone.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

**Example Code**

	BE_SMTPSend ( "me@me.com" ; "you@you.com" ; "Subject goes here" ; $textContent  ; $cc ; $bcc ; "replyto@me.com" ; $htmlContent ; "/path/to/doc.pdf¶/path/to/secondDoc.pdf" )

---

## BE_SMTPAddAttachment

```
BE_SMTPAddAttachment ( { attachment ; contentType } )
```

**Description**

Adds the details of the file in the *attachment* container field to the list of attachments, for future *BE_SMTPSend* functions, to be sent with the *contentType*.

**Parameters**

* *attachment* : A container field to use as an attachment in an email.
* *contentType* : A mime content type for the attachment.

**Version History**

* 3.3.0 : First Release
* 4.0.0 : Changed the *container* parameter to *attachment* and added the *contentType* parameter
* 4.0.2 : Renamed from BE_SMTP_AddAttachment

**Notes**

The *BE_SMTPSend* function lets you add attachments via a path whereas this function lets you select attachments from fields.  You can add multiple attachments by calling this function multiple times.  Once *BE_SMTPSend* is done it clears out the list of stored attachments regardless of the success of the Send operation.

Attachments are actually written to disk by the plugin, to the users temp folder, so this does require you to have access to a working temp folder ( defined by the OS ).

The *contentType* is what is set as the mime type for emails.  So usually something like :

	Content-Type: application/pdf; charset=UTF-8
	Content-Type: text/plain; name="file.txt"
	Content-Type: application/octet-stream; name="file.pdf"

Possible mime types are found here :

[https://www.w3docs.com/learn-html/mime-types.html](https://www.w3docs.com/learn-html/mime-types.html)

The result of adding the attachment may be unknown until after the *BE_SMTPSend* function has run.  Use the *BE_CurlTrace* function and look for the section where the file path is referenced and any resulting error messages.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

---

## BE_SMTPSetHeader

```
BE_SMTPSetHeader ( { header ; value } )
```

**Description**

Sets a custom SMTP header for subsequent SMTP send actions.

**Parameters**

* *header* ( optional ) : header name.
* *value* ( optional ) : header value.

You can remove all headers by calling with no parameters.

**Version History**

* 4.0.0 : First Release
* 4.0.2 : Renamed from BE_SMTP_Set_Header

**Notes**

A useful list of possible header values can be found here :

[https://www.iana.org/assignments/message-headers/message-headers.xhtml](https://www.iana.org/assignments/message-headers/message-headers.xhtml)

Some possible uses for this function are to apply headers such as :

	X-Priority: 1 (Highest)
	X-MSMail-Priority: High
	Importance: High

The actual effect of setting this header depends on the both the mail server and clients.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |
