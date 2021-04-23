# Troubleshooting
## Leaf client errors
#### Startup error: "Contact your Leaf administrator"
??? tldr "Answer"
    When installing Leaf and running the Leaf client app in your browser, you may run into some variation of this message:
    
    <p align="center"><img src="../images/err_server_not_found.png"/></p>

    > The particular message may instead be "*Hmmm... No Leaf server was found*", etc., but the best means of troubleshooting is the same either way.

    **These errors are a symptom of the browser client failing to communicate with the Leaf API.**

    **What to do:**

    1. **Check that your API is up-and-running** - the Leaf client will only give you vague, indirect information about where the failure lies (note that this is *by design* and largely for security reasons). There are a number of ways to check if your API is running, including `systemctl status` or `apachectl status` (if on Linux), the IIS user interface or Event Viewer (if on Windows), or `docker ps -a` / `podman ps -a` (if using containers). Choose the best method appropriate for your deployment.

        Perhaps most simply, **check the Leaf API logs**. These will tell you immediately whether the Leaf API successfully started, and/or any errors it has run into along with way. Remember that **Leaf API files are stored in the directory you set as your [SERILOG_DIR](../installation/installation_steps/7_env.md)**. This can be as simple as:

        ```sh
        $ tail -10 /$SERILOG_DIR/leaf-api-<todays_date>.log
        ```

        **If the API is not running**, dig into the logs to see what errors occurred. Often you may find that the API failed to communicate with the Leaf app database (hint: look for `"Error":"System.Data.SqlClient.SqlException"`). These cases are nearly always due to incorrect SQL passwords or usernames configured in your [environment variables](../installation/installation_steps/7_env.md).

        **If no log files appear and you are running IIS**, see [I started the Leaf API but don't see any log files!](#i-started-the-leaf-api-but-dont-see-any-log-files) below.

#### Visualization and Patient List error: "Whoops! An error occurred while loading patient visualizations"
??? tldr "Answer"
    When installing Leaf and running the Leaf client app in your browser, you may run into some variation of this message:
    
    <p><img src="../images/err_visualization.png"/></p>

    **These errors result from an error while the API is executing the [Basic Demographics query](../../administration/datasets/#basic-demographics)**

    **What to do:**

    1. **Check that you've [defined a Basic Demographis query](../../administration/datasets/#defining-the-basic-demographics-query)** - If you haven't yet, go ahead and do so, making sure your output column types match the expected column types.

    2. **Check the Leaf logs for execution or validation errors** - As ensuring de-identification and so on are performed correctly, **Leaf is very strict about output column types matching expected column types**. The Leaf logs will tell you of any SQL execution errors or column type mismatch errors that occur.

#### I'm trying to build the Leaf client but get the error, "Failed at...'react-scripts build'"
??? tldr "Answer"
    This is usually caused by older versions of [NPM](https://www.npmjs.com/) and [NodeJS](https://nodejs.org/en/) being used. We recommend using **NodeJS version 14.9.0+** and **NPM version 6.14.8+.**

    You can check your current versions using:

    ```sh
    $ node --version
    # v14.9.0
    
    $ npm --version
    # 6.14.8
    ```

    If you have an older version, the easiest way to upgrade is typically by using:

    ```sh
    $ npm upgrade -g
    ```

    See [this helpful article](https://www.geeksforgeeks.org/how-to-update-npm/) or [NPM documentation](https://docs.npmjs.com/cli/v6/commands/npm-update) for more information.

    **Alternatively**, you could instead also download the latest pre-compiled `leaf-ui-client-xxx.tar.gz` file from the [Leaf Releases page](https://github.com/uwrit/leaf/releases).

## Leaf API errors
#### I started the Leaf API but don't see any log files!
??? tldr "Answer"
    If you are running IIS:

    1. **Make sure your service account is running in IIS and has permissions to write** to the `SERILOG_DIR` directory.

    2. **Check the Windows Event Viewer** application for possible errors:
    <p align="center"><img src="../../installation/images/iis_event_viewer_error.png" /></p>

    3. **Check that your browser can communicate with the API** - If the API appears to be up, in a separate browser tab, go to `https://<your_leaf_url>/api/config`. 

        **If your browser returns an error with no data:**

        * **If the error is a 500**, then the API call *likely reached* the API, but there was an error of some kind. Check the logs to see what went wrong.

        * **If the error was a 40x**, then the API call *likely did not* reach the API, in which case your API is not listening or available as you'd expect it to be. In this case, check your [Apache](../installation/installation_steps/8a_configure_apache.md) or [IIS](../installation/installation_steps/8b_configure_iis.md) configuration to make sure everything is configured correctly. Note that each of these ([as well as Shibboleth](https://wiki.shibboleth.net/confluence/display/SP3/Logging)) include logging of their own which may be helpful.

#### API Error: "Do not run UNSECURED authentication in non-development environments!"
??? tldr "Answer"
    In a bit of well-intentioned caution, [Leaf assumes you will always want to authenticate your users](https://github.com/uwrit/leaf/blob/master/src/server/API/Options/StartupExtensions.Options.cs#L446). The reason for this is to prevent inadvertantly allowing access to identified patient information because users weren't authenticated. 
    
    While that's a precaution, we understand it can also be unhelpful if you're simply evaluating Leaf with synthetic/de-identified data, or only for a small number of users.

    To do so, as mentioned in [the Installation documentation](../../../installation/installation_steps/4_compile_api/#compilation) compile the API with:

    ```sh
    $ dotnet publish -c Debug ...<additional parameters>
    ```

    The **`-c Debug`** parameter compiles the Leaf API but relaxes the above requirement.

#### API Error: "Unable to start Kestrel.","Exception" : "System.Net.Sockets.SocketException (13) : Permission denied"
??? tldr "Answer"
    If you are deploying in a Linux environment, be sure the `ASPNETCORE_URLS` environment variable is set:

    ```sh
    ASPNETCORE_URLS=http://0.0.0.0:5001
    ```

#### API Error: "Value cannot be null. (Parameter 'path1')"
??? tldr "Answer"
    This occurs if the API is unable to read your [environment variables](../../../installation/installation_steps/7_env) correctly. Check to make sure those are configured and visible to the API.

#### API Error: "nameID header not found, no scoped identity available"
??? tldr "Answer"
    This error occurs if ***Shibboleth attributes*** aren't correctly mapped to ***user attributes***, and thus aren't sent in headers to the Leaf API. 

    > Both Apache and IIS use a remote user, which is a special trusted value populated by various mechanisms (`LOGON_USER`, `REMOTE_USER`) related to the attributes passed along.

    **What to do:**

    Take a look at the Shibboleth logs to see what is being sent in the SAML message (you may need debug mode set) when a user comes back from the login page and then update the `attribute-map.xml` such that the web server (Service Provider) has those details passed through to the app.
 
    If you’re aiming to follow standards, look to [InCommon’s Research and Scholarship](https://wiki.refeds.org/display/ENT/Research+and+Scholarship) area where they have expectations of the following:
 
    * On the ***releasing*** side, see [https://wiki.refeds.org/display/ENT/Research+and+Scholarship+IdP+Config](https://wiki.refeds.org/display/ENT/Research+and+Scholarship+IdP+Config)
    
    * On the ***receiving*** side, in `attribute-map.xml`:
    ```xml
    <!-- displayName -->
    <Attribute name="urn:mace:dir:attribute-def:displayName" id="displayName"/>
    <Attribute name="urn:oid:2.16.840.1.113730.3.1.241" id="displayName"/>

    <!-- email -->
    <Attribute name="urn:mace:dir:attribute-def:mail" id="mail"/>
    <Attribute name="urn:oid:0.9.2342.19200300.100.1.3" id="mail"/>
    ```