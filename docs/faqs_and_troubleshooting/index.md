# General Questions
### What makes Leaf unique compared to tools like i2b2 or TriNetX?
!!! tldr "Answer"
    This is a great question! While a detailed answer would be quite lengthy, here are a few distinguishing features of Leaf:

    - [x] **Open** - Leaf's codebase is open to the community, and you can use, change, and deploy Leaf as you'd like
    - [x] **Adaptable to any data model** - Leaf does not assume your data is structured in any particular way
    - [x] **Use on your terms** - Leaf never "*phones home*" and runs on your hardware. No data leaves your servers (besides exports from users, which can be disabled)
    - [x] **Free!** - We develop and use Leaf internally at the University of Washington and share with the community in the hopes of improving healthcare and collaboration
    - [x] **Modern and fun** - Leaf uses a cutting-edge development stack ([ReactJS](https://reactjs.org/), [TypeScript](https://www.typescriptlang.org/), [.NET Core](https://docs.microsoft.com/en-us/aspnet/core/introduction-to-aspnet-core?view=aspnetcore-5.0)) that allows for a fun, intuitive user experience that we believe meets or exceeds that of comparable commercial products (which you would *pay* for, either in software-as-a-service fees or by allowing external use of your data)


### I just want to test Leaf out. Is there an easy way to do so?
!!! tldr "Answer"
    Absolutely! Leaf can take time to install and wanting to "*kick the tires*" is understandable. Here are two options, depending on your preference:

    1. **If you would like to run Leaf on your local computer** or server and are able to run containers using Docker, the [containerize_leaf.sh script](https://github.com/uwrit/leaf/blob/master/containerize_leaf.sh) can be a useful time saver. The script functions similarly to a `docker-compose.yml` file and:

        * Deploys the Leaf API and SQL Server in a Docker container
        * Creates a `TestDB` with dummy clinical data you can query or build upon

        This allows you quickly run Leaf and decide whether it is right for you. Note that before running the script, you should [create a JWT signing key](../installation/installation_steps/3_jwt.md) and [set your environment variables](../installation/installation_steps/7_env.md) as you would with a "normal" Leaf deployment. 

        Afterward, to start the Leaf web client, run `npm start` from the `/leaf/src/ui-client` directory.

    2. **If you'd prefer jump in and try Leaf with synthetic data**, please contact us at [leafsupport@uw.edu](mailto:leafsupport@uw.edu), as we have several Leaf instances with synthetic data used for demonstration purposes and hosted at the University of Washington. While *we'd like* to have these available on the internet without making you jump through hoops, for various security reasons we ask that you just reach out to us and let us know you're interested, after which we'll grant you access.
