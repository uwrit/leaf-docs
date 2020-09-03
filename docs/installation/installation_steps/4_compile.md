# 4 - Compile the Leaf API
The Leaf API as available on GitHub is uncompiled, and must first be compiled in order to be used as a production deployment.

Alternatively, you may also download the latest pre-compiled Leaf API from the [Leaf Releases page on GitHub](https://github.com/uwrit/leaf/releases). If you download the pre-compiled version, skip ahead to [Deploying the API as a Service](#deploying-the-api-as-a-service).

## Compilation
There are a variety of ways to build the API, as the `dotnet` CLI tool supports both self-contained builds as well as runtime dependent targets. Although .NET Core is cross-platform, some targets have quirks that should be noted. If you're curious, you can review the [build.sh](https://github.com/uwrit/leaf/blob/master/build.sh) script in the project's root directory. Self-contained builds produce an executable and embed the entire .NET runtime in the build artifacts, resulting in a much larger deployment payload but removing the need to install the .NET Core runtime on your target machine. Conversely, runtime dependent builds assume that the .NET Core runtime will be installed on your target machine and only includes the application and its 3rd party dependencies in the artifact folder.

### Red Hat Enterprise Linux (RHEL)/CentOS
RHEL7 and Cent7 only support runtime dependent builds.

On RHEL7 or Cent7 with .NET Core installed:
```bash
dotnet publish -c Release -o <output_dir>
```

First, `cd` into where leaf source code was downloaded from github. Then targeting the example `/var/opt/leafapi/api` directory, the build command would be:

```bash
cd /var/opt/leaf/leaf_download/leaf/
dotnet publish -c Release -o /var/opt/leafapi/api
```

To build on Windows/MacOS building for RHEL/Cent:
```bash
dotnet publish -c Release -o <output_dir> -r rhel.7-x64 --self-contained false /p:MicrosoftNETPlatformLibrary=Microsoft.NETCore.App
```

### Windows
Windows fully supports both self-contained builds as well as runtime dependent builds.

Self-contained:
```bash
dotnet publish -c Release -r win-x64 -o <output_dir>
```
Runtime dependent:
```bash
dotnet publish -c Release -o <output_dir>
```

<br>
Next: [5 - Configure the appsettings file](../5_appsettings)