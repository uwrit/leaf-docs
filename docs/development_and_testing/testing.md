# Testing Leaf
Interesting in trying Leaf out? Great! Below are a few options, depending on your environment and preferences.

=== "Local Computer"

    === "MacOS"

        !!! note "These instructions assume you"
            - Are comfortable using bash or other Unix-based command line tools
            - Have [Docker installed](https://www.docker.com/) (to run the API and DB)
            - Have [npm installed](https://www.npmjs.com/get-npm) (to run the client)
            - Have [sqlcmd installed](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-setup-tools?view=sql-server-ver15#macos) (to populate the database)

        ---

        1. Complete the following steps in the [Leaf Installation Guide](../../installation/installation_steps/1_app_db):

            - [3 - Create a JWT Signing Key](../../installation/installation_steps/3_jwt)

            - [7 - Set Environment Variables](../../installation/installation_steps/7_env)

            !!! info "Environment Variables must be visible to the Docker containers run from the command line, though you can configure them however you'd like"

        2. Build and populate the Leaf API and databases. On the command line run:

            ```sh
            $ cd /leaf
            $ ./containerize_leaf.sh
            ```

            You should see output indicating the app and clinical database containers are up and populated with data, followed by the API:
            <p><img src="../images/testing_dotnet_run.png" /></p>

            !!! info "Alternatives to using containerize_leaf.sh"
                **Database** - If you have an existing SQL Server Docker container, you can simply build the app database as described in [1 - Create App Database](../../installation/installation_steps/1_app_db).

                ---

                **API** - You can either:
                
                - Manually build and run the [Leaf API Dockerfile](https://github.com/uwrit/leaf/blob/master/src/server/Dockerfile) (see [`./containerize_leaf.sh`](https://github.com/uwrit/leaf/blob/master/containerize_leaf.sh#L33) for an example of how to do this)

                *or*

                - If you prefer not to use Docker and have the [dotnet cli installed](https://docs.microsoft.com/en-us/dotnet/core/tools/), in bash, run:

                ```sh
                $ cd /leaf/src/server/API
                $ dotnet run
                ```

        3. In a separate command line window, run the Leaf client:

            ```sh
            $ cd /leaf/src/ui-client
            $ npm install
            $ npm start
            ```

            <p align="center"><img src="../../administration/images/concept_firstquery.gif"/></p>

            !!! success "Success! Leaf should start up in your browser using `http://localhost:3000`"

            !!! info "If you run into errors check the [Leaf Troubleshooting Guide](../../faqs_and_troubleshooting/troubleshooting)"

    === "Windows"

        !!! note "These instructions assume you"
            - Have SQL Server 2014+ installed and accessible by `localhost` with a clinical database available to test with
            - Have a Unix-like command line tool such as [Cygwin installed](https://www.cygwin.com/)
            - Have the [dotnet cli installed](https://docs.microsoft.com/en-us/dotnet/core/tools/) (to run the API)
            - Have [npm installed](https://www.npmjs.com/get-npm) (to run the client)

        ---

        1. Complete the following steps in the [Leaf Installation Guide](../../installation/installation_steps/1_app_db):

            - [1 - Create App Database](../../installation/installation_steps/1_app_db) (on `localhost`)

            - [3 - Create a JWT Signing Key](../../installation/installation_steps/3_jwt)

            - [7 - Set Environment Variables](../../installation/installation_steps/7_env)

            !!! info "Setting Environment Variables at the System level is usually simplest, though any method works as long as the Environment Variables are visible to the API"

        2. As your application and clinical datatases should now be running on SQL Server `localhost`, next we'll run the Leaf API. On the command line run:

            ```sh
            $ cd /leaf/src/server/API
            $ dotnet run
            ```

            You should see something like:
            <p><img src="../images/testing_dotnet_run.png" /></p>

        3. In a separate command line window, run the Leaf client:

            ```sh
            $ cd /leaf/src/ui-client
            $ npm install
            $ npm start
            ```

            !!! success "Success! Leaf should start up in your browser using `http://localhost:3000`"

            !!! info "If you run into errors check the [Leaf Troubleshooting Guide](../../faqs_and_troubleshooting/troubleshooting)"

        
=== "Remote Server"

    === "Linux"

        Coming soon!

    === "Windows"

        Coming soon!

=== "Access a UW Test Instance"

    Please contact us at [leafsupport@uw.edu](mailto:leafsupport@uw.edu), as we have several Leaf instances with synthetic data used for demonstration purposes and hosted at the University of Washington. While *we'd like* to have these available on the internet without making you jump through hoops, for various security reasons we ask that you just reach out to us and let us know you're interested, after which we'll grant you access.