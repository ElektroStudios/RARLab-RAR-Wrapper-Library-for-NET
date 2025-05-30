<!-- Common Project Tags:
command-line
console-applications
desktop-app
desktop-application
dotnet
dotnet-core
netcore
netframework
netframework48
tool
tools
vbnet
visualstudio
windows
windows-app
windows-application
windows-applications
windows-forms
winforms
ZIP
RAR
rar.exe
WinRAR
RARLab
Library
DLL
Wrapper
Compression
Compressed
Compress
Extraction
Extracted
Extract
Test
Testing
Listing
Archive
 -->

---
> This source code is freely distributed as part of the **"DevCase Class Library for .NET Developers"**.  
>  
> If you find this library useful, consider supporting my work by purchasing the full DevCase suite.  
> It includes a powerful set of APIs covering a wide range of features and development topics.  
>  
> 🔗 [Visit the purchase page here](https://github.com/ElektroStudios/DevCase.github.io)  
>  
> Thank you for your support! 🙏


# 📦 RARLab's `rar.exe` .NET Wrapper Library

A full-featured .NET wrapper for RARLab's official `rar.exe` command-line tool. This library empowers .NET developers to seamlessly access and control almost all the functionality provided by `rar.exe` — such as compressing, extracting, listing, testing, creating recovery volumes and managing RAR archives — from within their applications.

------------------

## 👋 Introduction

RARLab provides a powerful command-line tool, `rar.exe`, with deep support for creating and extracting RAR archives. However, using it directly from .NET requires manually spawning processes, constructing CLI arguments, and parsing the textual output — a repetitive and error-prone task.

This project eliminates that friction by wrapping a range of `rar.exe` functionalities in an intuitive .NET interface.

The wrapper is implemented in VB.NET but is fully compatible with both VB.NET and C# projects. It targets .NET Framework 4.8.1 by default, but can be easily migrated to .NET 5+ without issues, as it relies on no external dependencies other than the user-supplied `rar.exe` executable.

## 👌 Features

✅ Archive Creation and Modification
Create, fresh or update .rar files from one or multiple files/folders, with support for recursive inclusion, solid archives, volume splitting, and compression tuning.

✅ Extraction Support
Extract archives to custom locations, with full support for password-protected archives, overwrite rules, and directory flattening.

✅ Archive Listing
Retrieve file lists (size, timestamps, CRC, etc.) from existing archives programmatically.

✅ Password Support
Handle encrypted archives with ease, both when creating and extracting, using secure password input.

✅ Advanced RAR Options
Expose almost the full range of RAR’s features: recovery records, archive locking, comment injection, timestamps, file exclusions, and more.

✅ Multi-volume Archive Handling
Create and extract archives split into multiple volumes — ideal for large datasets or removable media.

✅ Error Handling and Output Parsing
Automatically parse output from `rar.exe`, detect errors, and raise structured exceptions where appropriate.

✅ Cross-language Usability
Though written in VB.NET, the wrapper is compiled into a .NET assembly (a dll library), making it easy to use in C#, F#, and other .NET-supported languages.

✅ No External Dependencies
Only dependency is `rar.exe` from RARLab — no third-party libraries required.

## 🧰 Requirements

- [`rar.exe`](https://www.rarlab.com/download.htm) from RARLab's WinRAR (command-line version only). 
  Please note that `rar.exe` is not free, it requires a product license (the same from a licensed `WinRAR` product).

## 🤖 Getting Started

Clone this repository or download the latest compilation by clicking [here](https://github.com/ElektroStudios/RARLab-RAR-Wrapper-Library-for-NET/releases/latest).

Add the project to your solution or reference the compiled DLL file.

Ensure `rar.exe` is present in your system environment path or specify its full path in the wrapper configuration. A working product license would also be required for using almost all the `rar.exe` functionalities.

Start compressing or extracting archives with simple method calls!

## 📝 Usage Examples

### Prepare a full featured command for RAR archive creation using the `RarCreationCommand` class:

VB NET:

    Imports DevCase.RAR
    Imports DevCase.RAR.Commands

    Dim archivePath As String = "C:\New Archive.rar"
    Dim filesToAdd As String = "C:\Directory to add\"
    
    Dim creationCommand As New RarCreationCommand(RarCreationMode.Add, archivePath, filesToAdd) With {
        .RarExecPath = ".\rar.exe",
        .RarLicenseData = "(Your license key)",
        .RecurseSubdirectories = True,
        .EncryptionProperties = Nothing,
        .SolidCompression = False,
        .CompressionMode = RarCompressionMode.Normal,
        .DictionarySize = RarDictionarySize.Mb__128,
        .OverwriteMode = RarOverwriteMode.Overwrite,
        .FilePathMode = RarFilePathMode.ExcludeBaseDirFromFileNames,
        .FileTimestamps = RarFileTimestamps.All,
        .AddQuickOpenInformation = TriState.True,
        .ProcessHardLinksAsLinks = True,
        .ProcessSymbolicLinksAsLinks = TriState.True,
        .DuplicateFileMode = RarDuplicateFileMode.Enabled,
        .FileChecksumMode = RarFileChecksumMode.BLAKE2sp,
        .ArchiveComment = New RarArchiveComment("Hello world!"),
        .RecoveryRecordPercentage = 0,
        .SplitIntoVolumes = Nothing,
        .FileTypesToStore = Nothing
    }

C#:

    using DevCase.RAR;
    using DevCase.RAR.Commands;

    string archivePath = "C:\\New Archive.rar";
    string filesToAdd = "C:\\Directory to add\\";
    
    RarCreationCommand creationCommand = new RarCreationCommand(RarCreationMode.Add, archivePath, filesToAdd) {
        RarExecPath = ".\\rar.exe",
        RarLicenseData = "(Your license key)",
        RecurseSubdirectories = true,
        EncryptionProperties = null,
        SolidCompression = false,
        CompressionMode = RarCompressionMode.Normal,
        DictionarySize = RarDictionarySize.Mb__128,
        OverwriteMode = RarOverwriteMode.Overwrite,
        FilePathMode = RarFilePathMode.ExcludeBaseDirFromFileNames,
        FileTimestamps = RarFileTimestamps.All,
        AddQuickOpenInformation = Microsoft.VisualBasic.TriState.True,
        ProcessHardLinksAsLinks = true,
        ProcessSymbolicLinksAsLinks = Microsoft.VisualBasic.TriState.True,
        DuplicateFileMode = RarDuplicateFileMode.Enabled,
        FileChecksumMode = RarFileChecksumMode.BLAKE2sp,
        ArchiveComment = new RarArchiveComment("Hello world!"),
        RecoveryRecordPercentage = 0,
        SplitIntoVolumes = null,
        FileTypesToStore = null
    };

### To retrieve the full command-line arguments of our Command:

    Console.WriteLine($"Command-line arguments: {creationCommand}")

### Execute the command using the `RarCommandExecutor` class:

VB NET:

    Using rarExecutor As New RarCommandExecutor(creationCommand)
    
        AddHandler rarExecutor.OutputDataReceived,
            Sub(sender As Object, e As DataReceivedEventArgs)
                Console.WriteLine($"[Output] {Date.Now:yyyy-MM-dd HH:mm:ss} - {e.Data}")
            End Sub
    
        AddHandler rarExecutor.ErrorDataReceived,
            Sub(sender As Object, e As DataReceivedEventArgs)
                If e.Data IsNot Nothing Then
                    Console.WriteLine($"[Error] {Date.Now:yyyy-MM-dd HH:mm:ss} - {e.Data}")
                End If
            End Sub
    
        AddHandler rarExecutor.Exited,
            Sub(sender As Object, e As EventArgs)
                Dim pr As Process = DirectCast(sender, Process)
                Dim rarExitCode As RarExitCode = DirectCast(pr.ExitCode, RarExitCode)
                Console.WriteLine($"[Exited] {Date.Now:yyyy-MM-dd HH:mm:ss} - rar.exe process has terminated with exit code {pr.ExitCode} ({rarExitCode})")
            End Sub
    
        Dim exitcode As RarExitCode = rarExecutor.ExecuteRarAsync().Result
    End Using

C#:

    using (RarCommandExecutor rarExecutor = new RarCommandExecutor(creationCommand)) {
    
      rarExecutor.OutputDataReceived += (object sender, DataReceivedEventArgs e) => {
          Console.WriteLine($"[Output] {DateTime.Now:yyyy-MM-dd HH:mm:ss} - {e.Data}");
      };
    
      rarExecutor.ErrorDataReceived += (object sender, DataReceivedEventArgs e) => {
          if (e.Data != null)
          {
            Console.WriteLine($"[Error] {DateTime.Now:yyyy-MM-dd HH:mm:ss} - {e.Data}");
          }
      };
    
      rarExecutor.Exited += (object sender, EventArgs e) => {
          Process pr = (Process)sender;
          RarExitCode rarExitCode = (RarExitCode)pr.ExitCode;
          Console.WriteLine($"[Exited] {DateTime.Now:yyyy-MM-dd HH:mm:ss} - rar.exe process has terminated with exit code {pr.ExitCode} ({rarExitCode})");
      };
    
      RarExitCode exitcode = rarExecutor.ExecuteRarAsync().Result;
    }

## 🔄 Change Log

Explore the complete list of changes, bug fixes, and improvements across different releases by clicking [here](/Docs/CHANGELOG.md).

## ⚠️ Disclaimer:

This Work (the repository and the content provided in) is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose and noninfringement. In no event shall the authors or copyright holders be liable for any claim, damages or other liability, whether in an action of contract, tort or otherwise, arising from, out of or in connection with the Work or the use or other dealings in the Work.

This Work has no affiliation, approval or endorsement by RARLab, the registered company and trademark owner of `rar.exe`.

## 💪 Contributing

Your contribution is highly appreciated!. If you have any ideas, suggestions, or encounter issues, feel free to open an issue by clicking [here](https://github.com/ElektroStudios/RARLab-RAR-Wrapper-Library-for-NET/issues/new/choose). 

Your input helps make this Work better for everyone. Thank you for your support! 🚀

## 💰 Beyond Contribution 

This work is distributed for educational purposes and without any profit motive. However, if you find value in my efforts and wish to support and motivate my ongoing work, you may consider contributing financially through the following options:

<br></br>
<p align="center"><img src="/Images/github_circle.png" height=100></p>
<p align="center">__________________</p>
<h3 align="center">Becoming my sponsor on Github:</h3>
<p align="center">You can show me your support by clicking <a href="https://github.com/sponsors/ElektroStudios/">here</a>, <br align="center">contributing any amount you prefer, and unlocking rewards!</br></p>
<br></br>

<p align="center"><img src="/Images/paypal_circle.png" height=100></p>
<p align="center">__________________</p>
<h3 align="center">Making a Paypal Donation:</h3>
<p align="center">You can donate to me any amount you like via Paypal by clicking <a href="https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=E4RQEV6YF5NZY">here</a>.</p>
<br></br>

<p align="center"><img src="/Images/envato_circle.png" height=100></p>
<p align="center">__________________</p>
<h3 align="center">Purchasing software of mine at Envato's Codecanyon marketplace:</h3>
<p align="center">If you are a .NET developer, you may want to explore '<b>DevCase Class Library for .NET</b>', <br align="center">a huge set of APIs that I have on sale. Check out the product by clicking <a href="https://codecanyon.net/item/elektrokit-class-library-for-net/19260282">here</a></br><br align="center"><i>It also contains all piece of reusable code that you can find across the source code of my open source works.</i></p>
<br></br>

<h2 align="center"><u>Your support means the world to me! Thank you for considering it!</u> 👍</h2>
