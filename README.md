# Rewriter

> Long-running Windows background service for automatic file conversion to PDF.
> Built entirely in C# with observables, DI, and modern .NET patterns.

---

## Features

- **Background worker service**
  - Long-running Windows application monitoring input folders for file changes.
- **Supported input formats**
  - **Presentation:** `.ppa`, `.ppt`, `.pptm`, `.pptx`
  - **Document:** `.doc`, `.docm`, `.docx`, `.htm`, `.html`
- **Logging**
  - Configurable logger
  - Supports file-based logs with separate files per run and customizable log levels.

---

## Architecture & Tech

- **Language & Framework:** C# (.NET 8.0)
- **Service Type:** Long-running background service built on the .NET Worker Service template
- **Event-driven:** Observables wrapping `FileSystemWatcher` events for reactive file monitoring
- **Conversion Engine:**
  - Uses `Microsoft.Office.Interop` for document and presentation to PDF conversion
  - Converter and watcher instances are created using the **Factory Pattern** for modularity and flexibility
  - Custom extension attributes are used to manage different file formats
- **Configuration & Validation:**
  - .NET Configuration API with Options pattern
  - FluentValidation ensures user-supplied options are valid
- **Hosting & Logging:**
  - Runs as a Windows service via `Microsoft.Extensions.Hosting`
  - Logging is configurable with file separation and adjustable log levels

## Status

This project is **finished** and ready for use as a background service.  
It is designed for stability, automation, and production use.

---

## Installation

### Build

#### Option A: Framework-dependent build
Requires **.NET 8 Runtime** on the target machine.  

```bash
  dotnet build "C:\Path\Rewriter\Rewriter.csproj" -c Release -o "C:\Deploy\MyService"
```

#### Option B: Self-contained build
Includes all required runtime files.  

```bash
  dotnet publish "C:\Path\Rewriter\Rewriter.csproj" -c Release -r win-x64 -o "C:\Deploy\MyService"
```

### Install as a Windows Service

```bash
  sc.exe create MyService binPath= "C:\Deploy\MyService.exe"
```

---

## Configuration

The service is configured via `appsettings.json`.  

### File Input
- **DeleteOldFile** – remove input file after successful conversion.  
- **FileInputList** – list of sources to monitor:  
  - **FolderPaths** – folders to watch.  
  - **Extensions** – file extensions to convert (e.g., `.docx`, `.pptx`).  

### File Output
- **FolderPath** – destination folder for converted `.pdf` files.  

### Logging
- **FolderPath** – directory where log files are written.  
- **UseSeparateFiles** – when true, creates separate log files.  
- **LogLevel** – minimum log level (`Information`, `Warning`, `Error`).  
- **FileLogger** – specific logging rules for file logging.  

---

## Usage

After building and installing the service:  

### Start the service

```bash
  sc.exe start MyService
```

### Stop the service

```bash
  sc.exe stop MyService
```

The service will now monitor the configured folders, convert matching files to PDF, and write logs according to your `appsettings.json`.

---

## License

Unlicensed — personal or internal use only.
