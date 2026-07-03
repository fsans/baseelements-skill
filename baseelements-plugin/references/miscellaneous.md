# Miscellaneous — BaseElements Plugin

## BE_ExecuteSystemCommand

```
BE_ExecuteSystemCommand ( command ; { timeout ; executeUsingShell } )
```

**Description**

Performs a command line script of the *command* parameter. Similar to using the same command using Terminal on the Mac, or CMD.exe on Windows, but with **lots of caveats** as to the differences ( see Notes below ).

**Parameters**

* *command* : The command to run.
* *timeout* ( optional, default:empty ) : A value, in milliseconds that the plugin should wait for a result before returning.  No value specified will wait indefinitely ( not recommended ) and a value of 0 will return immediately.
* *executeUsingShell* ( optional, default:True ) : whether to execute the command directly or in a shell.

**Version History**

* 2.0.0 : First Release
* 4.0.6 : Switched to a different library to perform the command, which broke a lot of things, and meant it could only run one command at a time
* 4.1.3 : Do not crash when attempting to execute an empty command.
* 4.1.3 : macOS, Linux and iOS enable multiple calls to shell like version 4.0.5
* 4.2.0 : Reverted back to the 4.0.5 option and fixed the crash issue, and added executeUsingShell as an option

**Notes**

You can string multiple commands together in a single plugin function call by using Char ( 10 ) on Mac and Char ( 13 ) on Windows as command separators, but only when using *executeUsingShell* set to True.

**This function call does not behave the same as when running the same command in Terminal or CMD.exe.**

Some things to note :

This function will not start with the same default location as a normal terminal command.

This function will not always run as the same user as the normal shell, especially when running on Server where the process runs as the fmserver user, so permissions may be different between this command and the terminal.  For example, you may have access to read the location to an executable, but not to run it.  Or permission to run it but not to access the data you send to it.

This function may not have access to default environment variables as a normal user, so things like paths to commands may not exist.  Run the *printenv* command from the terminal and also from the BE plugin and compare the two, to see if there are differences.

Escaping quotes on Windows is complex and fraught with issues.  The most common issue people have with running commands is how to escape quotes so that they appear in the right place in the command you're running.  However quotes on windows are not simple, and we recommend people read this for more info :

[https://stackoverflow.com/questions/355988/how-do-i-deal-with-quote-characters-when-using-cmd-exe](https://stackoverflow.com/questions/355988/how-do-i-deal-with-quote-characters-when-using-cmd-exe)

Using the | pipe character on windows is known to not work.

This function returns anything send to stdout and everything else needs to be captured by the command you're running.

---

Given all of that, if you're unable to get a command to run the issue is likely to do with what the command is trying to do, and the complexities of users, permissions, paths, errors, encoding, quoting, and not to do with the implementation of this function.  It's unlikely that we can fix your function call.

If you're copying and pasting a command from somewhere and expecting it to work without understanding the underlying intricacies of the operating system and it's shell, you'l have a hard time trying to debug your command.  Especially if the command "works" when you run it manually outside of the plugin.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | No |
| Linux | Yes |

---

## BE_EvaluateJavaScript

```
BE_EvaluateJavaScript ( javaScript )
```

**Description**

Evaluates the JavaScript passed to it as text and returns a result, if any.

**Parameters**

* *javaScript* : the JavaScript to evaluate.

**Version History**

* 3.1.0 : First Release

**Notes**

There are also two callback functions, so you can call FileMaker calculations using BE_Evaluate_FileMaker_Calculation or run FileMaker Script using BE_ExecuteScript from within the JavaScript code.

Marked as **Testing** as it's not widely used, and functionality may change in the future, or even be removed.

This function uses the [Duktape](https://duktape.org) library.  We have not had many reports of people using this library in useful ways, so it may become deprecated in a future release.

The other direction this library could go is that we use it to add new built in BE plugin functions, that are purely js run via Duktape - we're open to ideas and suggestions for options here.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Testing |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | No |
| iOS | No |
| Linux | No |

---

## BE_GetMachineName

```
BE_GetMachineName
```

**Description**

This function just returns the result of the %COMPUTERNAME% variable on Windows, or gets the computer hardware identifier if the operating system supports it.

**Parameters**

None

**Version History**

* 4.0.0 : First Release

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

## BE_GetSystemDrive

```
BE_GetSystemDrive
```

**Description**

The same effect as the internal FileMaker function Get ( SystemDrive ) but because it's plugin based will also function on FileMaker Server where the internal function does not work.

**Parameters**

None

**Version History**

* 4.1.0 : First Release

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

## BE_Pause

```
BE_Pause ( milliseconds )
```

**Description**

Pauses ( within the plugin code, so doesn't return to FMP ) for *milliseconds*.

**Parameters**

* *milliseconds* : The number of milliseconds to wait for.

**Version History**

* 3.3.0 : First Release

**Notes**

The plugin code runs in a single thread, so this will lock up any running script for this amount of time.

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

## BE_Version

```
BE_Version
```

**Description**

Returns the plugin version number as text, eg 1.1.3

**Parameters**

None

**Version History**

* 1.0.0 : First Release

**Notes**

This function should be not be used to when checking for latest updates, as the text string response of it is not easily compared. For example :

```
"1.9.0" < "1.10.0"
```

even though 10 is greater than 9.  To compare versions in order of release, use *GetAsNumber ( BE_VersionAutoUpdate )* instead and then use *BE_Version* to show a meaningful result to the end user.

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

## BE_VersionAutoUpdate

```
BE_VersionAutoUpdate
```

**Description**

The text of the plugin number, formatted as an 8 digit number, eg 04020400. This is broken into four parts, and so 4.2.4 would become 04020400 as 04 02 04 00. The final two digits come from a minor rebuild version, if there was one, when we did new builds but didn't change version number. This may be due to build issues or signing updates etc.

**Parameters**

None

**Version History**

* 1.0.0 : First Release

**Notes**

This function should be used in place of *BE_Version* when checking for latest updates and comparing versions as numbers, as the text string response of *BE_Version* is not as easily compared. Use *BE_Version* when showing a meaningful description to the end user.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |
