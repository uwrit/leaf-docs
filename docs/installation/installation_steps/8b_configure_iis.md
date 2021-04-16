# 8b - Configure Leaf with IIS

![Infra](../images/infra_iis_focus.png "Architecure-Focus-Example") 

The following IIS guide assumes you are using a combined web & app single server to both host the Leaf API and handle user traffic.

**On the web/app server**:

1. Install the [.NET Core Hosting Bundle](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/?view=aspnetcore-2.2)
2. Install [IIS URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite)
3. Install [Shibboleth Service Provider 3](https://wiki.shibboleth.net/confluence/display/SP3/Install+on+Windows#InstallonWindows-Installation)

    !!! info "Be sure to check `Configure IIS7 Module` box during installation."

4. Create an Application pool to run the site and API.
    <p align="center"><img src="../../images/iis_app_pool.png" /></p>

5. In the new Application pool's Advanced Settings, set `Load User Profile` to `true` and recycle the pool.

6. Create the website to host the Leaf browser application.
    <p align="center"><img src="../../images/iis_website.png" /></p>

7. Create an application behind the site to host the API.
    <p align="center"><img src="../../images/iis_api.png" /></p>

    !!! warning "Do NOT name the API application `'api'`, as this will cause the rewrite rule to apply recursively until the request fails. At UW we name the backing application `'leafapi'`"
   
8. Create a URL rewrite rule on the site with the following template.
    <p align="center"><img src="../../images/iis_url_rewrite.png" /></p>


    !!! warning "Be sure the `Append query string` box is checked. If not, API calls for Concept search will fail."

    Your **`web.config`** should now look like this:

    ```xml
    <system.webServer>
        ...additional configuration
        <rewrite>
            <rules>
                <rule name="add {applicationName}">
                    <match url="^(api/.*)" />
                    <action type="Rewrite" url="{applicationName}/{R:0}" appendQueryString="true" logRewrittenUrl="true" />
                </rule>
            </rules>
        </rewrite>
    </system.webServer>
    ```

9. If the WebDAV module is installed in your IIS instance, you need to either uninstall it or disable it for this site. It inteferes with PUT/DELETE HTTP verbs.

    ```xml
    <system.webServer>
        <modules runAllManagedModulesForAllRequests="true">
            <remove name="WebDAVModule" />
        </modules>
        ...additional configuration
    </system.webServer>
    ```

10. If you have not yet created the environment variables for IIS (as described in [Step 7 - Set Environment Variables](../7_env)), do so now.

<br>
Next: [Step 9 - Configure Authentication with SAML2](../9_saml2)