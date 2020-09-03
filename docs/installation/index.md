# Architecture
Leaf is designed to be deployed in a standard <a href="https://en.wikipedia.org/wiki/Multitier_architecture" target="_blank">three-tier architecture</a> with two deployment models, depending on your environment:

`Option 1`: **3-tier architecture with Linux and Apache**
![Single Instance Split](images/single_instance_split_server.png "Single Instance Split") 

or alternatively:

`Option 2`: **Combined 2-tier architecture with Windows and IIS**
![Single Instance Combined](images/single_instance_combined_server.png "Single Instance Combined") 

## Components
1. **Database Server**, with
    - The clinical database you'd like to point Leaf at deployed in <a href="https://www.microsoft.com/en-us/sql-server/default.aspx" target="_blank">MS SQL Server</a> (2014+).
    - <a href="https://github.com/uwrit/leaf/blob/master/src/db/build/LeafDB.sql" target="_blank">Leaf application database</a>. Note that this *must* be the same server the clinical database ↑ is deployed to.

2. **Application Server**, with
    - <a href="https://dotnet.microsoft.com/download" target="_blank">.NET Core Runtime</a> installed.

3. **Web Server**, with
    - <a href="https://en.wikipedia.org/wiki/Apache_HTTP_Server" target="_blank">Apache</a> or <a href="https://www.iis.net/overview" target="_blank">IIS</a> installed to handle <a href="https://en.wikipedia.org/wiki/HTTPS" target="_blank">https</a> routing for requests from the <a href="https://github.com/uwrit/leaf/tree/master/src/ui-client" target="_blank">client app</a>. These can be configured to work with a <a href="https://en.wikipedia.org/wiki/SAML_2.0" target="_blank">SAML2</a> Identity Provider to manage user authentication and authorization, such as <a href="https://www.shibboleth.net/index/" target="_blank">Shibboleth</a> or <a href="https://docs.microsoft.com/en-us/windows-server/identity/active-directory-federation-services" target="_blank">ADFS</a>.

<br>
Whether you use 2 or 3 servers, however, in general the deployment strategies are largely the same. **For simplicity, in this guide we'll illustrate examples using a 3-tier architecture, but all steps are applicable to either deployment model** unless stated otherwise.

<br>
Next: [Step 1 - Create the Leaf Application Databse](./installation_steps/1_app_db)