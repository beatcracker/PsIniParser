# PsIniParser

This module allows to import, export and convert INI files (and strings) to hashtables (or objects) and vice versa. You can specify various parsing options (INI files are not standardized), or use specific encoding while reading a file.

# Features

  * Provided cmdlets mimic native PowerShell ones (`Import-*`, `Export-*`, `ConvertTo-*`, `ConvertFrom-*`).
  * Uses actively supported [INI File Parser](https://github.com/rickyah/ini-parser) by Ricardo Amores Hernández.
  * Highly configurable, can read and write non-standard INI files
  * Supports any available .Net encoding for reading and writing files (unlike Windows' native functions and their PInvoke wrappers that have [very limited Unicode support](http://www.siao2.com/2006/09/15/754992.aspx).)
  * Full comment-based help and usage examples.

# Usage examples

## Import

* Single file

```powershell
'C:\Windows\System.ini' | Import-Ini
```
* Multiple files

```powershell
'C:\Windows\System.ini', 'C:\Windows\Win.ini' | Import-Ini
```

* All INI files in the directory

```powershell
'C:\Windows\*.ini' | Get-ChildItem | Import-Ini
```

* Single file with non-standard structure

```
{Section}
Key@Value
%Comment
```

```powershell
'Weird.ini' | Import-Ini -CommentStrings '%' -SectionStartChar '{' -SectionEndChar '}' -KeyValueAssigmentChar '@'
```  

## Convert from

* INI string

```powershell
"[Section]`nKey=Value" | ConvertFrom-Ini
```

* Multiple INI strings

```powershell
"[Section]`nKeyA=ValueA", "[SectionB]`nKeyB=ValueB" | ConvertFrom-Ini
```

* Single INI string with non-standard structure

```powershell
"{Section}`nKey@Value`n%Comment" | ConvertFrom-Ini -CommentStrings '%' -SectionStartChar '{' -SectionEndChar '}' -KeyValueAssigmentChar '@'
```

## Export

* Hashtable to INI file

```powershell
@{Section = @{Key = 'Value'}} | Export-Ini -Path '.\My.ini'
```

* Merge multiple hashtables to INI file

```powershell
@{SectionA = @{KeyA = 'ValueA'}}, @{SectionB = @{KeyB = 'ValueB'}} | Export-Ini -Path '.\My.ini'
```

* Hashtable to INI file with non-standard structure

```powershell
@{Section = @{Key = 'Value'}} | Export-Ini -Path '.\My.ini' -SectionStartChar '{' -SectionEndChar '}' -KeyValueAssigmentChar '@'
```

## Convert to

* Hashtable to INI string

```powershell
@{Section = @{Key = 'Value'}} | ConvertTo-Ini
```

* Multiple hashtables to INI string(s)

```powershell
@{SectionA = @{KeyA = 'ValueA'}}, @{SectionB = @{KeyB = 'ValueB'}} | ConvertTo-Ini
@{SectionA = @{KeyA = 'ValueA'}}, @{SectionB = @{KeyB = 'ValueB'}} | ConvertTo-Ini -Merge
```

* Hashtable to INI string with non-standard structure

```powershell
@{Section = @{Key = 'Value'}} | ConvertTo-Ini -SectionStartChar '{' -SectionEndChar '}' -KeyValueAssigmentChar '@'
```

# Installation

Download\clone this repository and it to your PowerShell modules folder. If you downloaded repository as ZIP file, you need to unblock it first:

* Using GUI: right-click ZIP file, click `Properties` and then click the `Unblock` button.
* Using PowerShell: `Unblock-File 'X:\Path\to\PsIniParser-master.zip'`

PowerShell will look in the paths specified in the `$env:PSModulePath` environment variable when searching for available modules on a system. Default locations are:

  * System-wide:
    * `C:\Program Files\WindowsPowerShell\Modules`
    * `C:\Windows\system32\WindowsPowerShell\v1.0\Modules\`
  * Per-user:
    * `C:\Users\USERNAME\Documents\WindowsPowerShell\Modules`

Modules stored in those locations are easily discoverable and autoloaded with PowerShell 3.0 and higher. If you're not sure, copy module to your *Per-user* folder.

* To list all available modules, use

```powershell
Get-Module -ListAvailable
```

* To import available module, use

```powershell
Import-Module -Name 'PsIniParser'
```

Alternatively, you can import module from any location

```powershell
Import-Module -Name 'X:\Path\to\PsIniParser'
```
