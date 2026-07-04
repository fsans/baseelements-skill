# Preferences — BaseElements Plugin

## BE_PreferenceSet

```
BE_PreferenceSet ( key ; value ; { domain } )
```

**Description**

Sets the preference value with the *key* in the preferences file located at the *domain*.

Preferences set via the BE plugin are available across all open copies of FileMaker applications.

**Parameters**

* *key* : the key code for the value to set.
* *value* : the value to store in this key.
* *domain* ( optional, default: See Notes ) : the domain value of where to locate the preference file.

**Version History**

* 1.3.0 : First Release
* 4.0.0 : Return error 3, command not available on Linux
* 4.0.2 : Renamed from BE_SetPreference

**Notes**

If the domain is not included, there is a default used on Mac and on iOS : 

	au.com.goya.baseelements.plugin-user
	
On Windows the default will be :

	Software\\Goya\\BaseElements\\PluginUser
	
( stored inside HKEY_CURRENT_USER ).

On Linux this function does not run, will return error 3 - Command is unavailable.

You can override the domain to specify a different file name on the mac, or a different path on Windows. To respect the various OS conventions you should use something similar to the above, but tailored to your solution.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | No  |  

---

## BE_PreferenceGet

```
BE_PreferenceGet ( key ; { domain } )
```

**Description**

Gets the preference value stored by BE_PreferenceSet at *key*, in *domain*, from the system.

Preferences set via the BE plugin are available across all open copies of FileMaker applications.

**Parameters**

* *key* : the key code for the value to get.
* *domain* ( optional, default: See Notes ) : the domain value of where to locate the preference file.

**Version History**

* 1.3.0 : First Release
* 4.0.0 : Return error 3, command not available on Linux
* 4.0.2 : Renamed from BE_GetPreference

**Notes**

If the domain is not included, there is a default used on Mac and on iOS : 

	au.com.goya.baseelements.plugin-user
	
On Windows the default will be :

	Software\\Goya\\BaseElements\\PluginUser
	
( stored inside HKEY_CURRENT_USER ).

On Linux this function does not run, will return error 3 - Command is unavailable.

You can override the domain to specify a different file name on the mac, or a different path on Windows. To respect the various OS conventions you should use something similar to the above, but tailored to your solution.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | No  |  

---

## BE_PreferenceDelete

```
BE_PreferenceDelete ( key ; { domain } )
```

**Description**

The is the reverse of the BE_PreferenceSet function, this removes the preference at *key*, in *domain* from the system.

Preferences set via the BE plugin are available across all open copies of FileMaker applications.

**Parameters**

* *key* : the key code for the value to set.
* *domain* ( optional, default: See Notes ) : the domain value of where to locate the preference file.

**Version History**

* 4.1.0 : First Release

**Notes**

If the domain is not included, there is a default used on Mac and on iOS : 

	au.com.goya.baseelements.plugin-user
	
On Windows the default will be :

	Software\\Goya\\BaseElements\\PluginUser
	
( stored inside HKEY_CURRENT_USER ).

On Linux this function does not run, will return error 3 - Command is unavailable.

You can override the domain to specify a different file name on the mac, or a different path on Windows. To respect the various OS conventions you should use something similar to the above, but tailored to your solution.

This function is required in some situations as the user can't directly delete preferences files to remove a stored preference.  For example on the Mac, Setting a preference will create a corresponding file in the Users Library/Preferences folder.  However deleting that file does not delete this preference until logout, and it's possible that the preference file will be recreated from memory.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | No  |  
