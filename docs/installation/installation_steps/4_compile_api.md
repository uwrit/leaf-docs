# 4 - Compile the Leaf API

![Infra](../images/infra_app_focus.png "Architecure-Focus-Example") 

The Leaf API as available on GitHub is uncompiled, and must first be compiled in order to be used as a production deployment.

!!! info
    Rather than compiling yourself, however, you may instead download the latest pre-compiled Leaf API from the [Leaf Releases page on GitHub](https://github.com/uwrit/leaf/releases). If you download the pre-compiled version, skip ahead to [5 - Compile the Leaf UI](../5_compile_client).

## Compilation
There are a variety of ways to build the API, as the `dotnet` CLI tool supports both self-contained builds as well as runtime dependent targets. Although .NET Core is cross-platform, some targets have quirks that should be noted. If you're curious, you can review the [build.sh](https://github.com/uwrit/leaf/blob/master/build.sh) script in the project's root directory. Self-contained builds produce an executable and embed the entire .NET runtime in the build artifacts, resulting in a much larger deployment payload but removing the need to install the .NET Core runtime on your target machine. Conversely, runtime dependent builds assume that the .NET Core runtime will be installed on your target machine and only includes the application and its 3rd party dependencies in the artifact folder.

---
From `/leaf_download/src/server`: 

=== "Linux (RHEL/CentOS/Ubuntu)"

    === "With User Authentication"

        ```sh
        $ dotnet publish -c Release -o /var/opt/leafapi/api
        ```

    === "No Authentication (UNSECURED)"

        ```sh
        $ dotnet publish -c Debug -o /var/opt/leafapi/api
        ```

=== "Windows"

    === "With User Authentication"

        ```sh
        $ dotnet publish -c Release -o <your_api_deployment_directory>
        ```

    === "No Authentication (UNSECURED)"

        ```sh
        $ dotnet publish -c Debug -o <your_api_deployment_directory>
        ```
---

!!! info "For more information on the `dotnet publish` command see [https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-publish](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-publish)"

!!! warning "Release vs. Debug compilation"
    Besides running Leaf less efficiently and using additional memory, `-c Debug` allows Leaf to run in `UNSECURED` [authentication](../6_appsettings#authentication) and [authorization](../6_appsettings#authorization) mode in a deployment setting. We recommend being **very careful** about this and **not using identified data** in this case, as **Leaf will assume all users to be administrators**

## Wrapping up

At this point, your Leaf directory on the app server should look similar to this:

```
var
├── opt
│   ├── leafapi
│   │   ├── keys                 # JWT signing key
│   │   ├── api                  # Compiled API
│   │   │   ├── runtimes         ########################
│   │   │   ├── API.deps         #
│   │   │   ├── API.dll          # Production build files
│   │   │   ├── appsettings.json #
│   │   │   ├── ...              ########################
│   │   ├── leaf_download        # Downloaded source files from GitHub
├── log
│   ├── leaf                     # Log files
```

<br>
Next: [5 - Build the Leaf UI](../5_compile_client)