# 2 - Configure App Server

![Infra](../images/infra_app_focus.png "Architecure-Focus-Example") 

The application server hosts the <a href="https://github.com/uwrit/leaf/tree/master/src/server" target="_blank">Leaf API</a>, and serves as the intermediary between the <a href="https://github.com/uwrit/leaf/tree/master/src/ui-client" target="_blank">client app</a> and <a href="https://github.com/uwrit/leaf/tree/master/src/db" target="_blank">databases</a>. The API is written in C# and .NET Core, and can run in either Linux or Windows environments. 

Note that the Leaf app database created in [Step 1 - Create App Database](../1_app_db) **must be query-able from this server**, so check to make sure any necessary firewall exceptions are in place.

We'll be using the following directory layout for organizing the API deployment:  

```
var
├── opt
│   ├── leafapi
│   │   ├── keys           # JWT signing key
│   │   ├── api            # Compiled API
│   │   ├── leaf_download  # Downloaded source files
├── log
│   ├── leaf               # Log files
```

Provided that you have a GitHub account and have setup your SSH key, start by creating the `leafapi` directory and downloading the source with git:

```bash
$ mkdir /var/opt/leafapi/leaf_download
$ cd    /var/opt/leafapi/leaf_download
$ git clone git@github.com:uwrit/leaf.git
```

## Prerequisites

### Linux
CentOS requires .NET to be installed prior to building the application. Refer to Microsoft's current instructions for installing .NET Core framework.
<a href="https://dotnet.microsoft.com/download/linux-package-manager/rhel/sdk-current" target="_blank">https://dotnet.microsoft.com/download/linux-package-manager/rhel/sdk-current</a>.


Currently, installing .NET on CentOS/RHEL looks like this:
```bash
$ rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm
$ yum install -y dotnet-sdk-2.2
```

### Windows
Ensure that IIS8+ is installed on the intended app server. See instructions at <a href="https://docs.microsoft.com/en-us/iis/get-started/whats-new-in-iis-8/installing-iis-8-on-windows-server-2012" target="_blank">https://docs.microsoft.com/en-us/iis/get-started/whats-new-in-iis-8/installing-iis-8-on-windows-server-2012</a>.


<br>
Next: [Step 3 - Create a JWT Signing Key](../3_jwt)