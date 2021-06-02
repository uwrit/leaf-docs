# 7 - Set Environment Variables

![Infra](../images/infra_app_focus.png "Architecure-Focus-Example") 

Leaf uses environment variables to store sensitive information, such as connection strings, which are loaded when the API is launched. Regardless of the OS and configuration, as a best practice we recommend using environment variables specific to the user account running the Leaf API (rather than global environment variables).

**Leaf environment variables with example values**:

```python
# Required
LEAF_JWT_CERT   = /var/opt/leafapi/.keys/cert.pem 
LEAF_JWT_KEY    = /var/opt/leafapi/.keys/leaf.pfx 
LEAF_JWT_KEY_PW = <your_pass>                     
LEAF_APP_DB     = <leaf_app_db_connection_string> 
LEAF_CLIN_DB    = <clinical_db_connection_string> 
SERILOG_DIR     = /var/log/leaf/                  
ASPNETCORE_URLS = http://0.0.0.0:5001             # Only if using Linux

# Optional
LEAF_REDCAP_SUPERTOKEN = <your_token>     # Only if using REDCap import/export
LEAF_SMTP_USR          = <smtp_user>      # Only if auto-generating help emails
LEAF_SMTP_PW           = <smtp_password>  # Only if auto-generating help emails
```

!!! info "SQL Connection string formatting"
    The **`LEAF_APP_DB`** and **`LEAF_CLIN_DB`** environment variables should be of the form:
    ```
    Server=<server>;Database=<dbname>;uid=sa;Password=<dbpassword>;
    ```

---
=== "Linux (RHEL/CentOS/Ubuntu)"

    If using Linux we recommend:

    1. Using full paths when referencing locations on the filesystem.
    2. Storing environment variables in a `.conf` file.

=== "Windows"

    In IIS environment variables are typically defined as configuration items in your backing application in IIS itself:

    ![IIS Environment Variables Navigation](../images/iis_environ.png "IIS Environment Variables Navigation")

    ![IIS Environment Variable Entries](../images/iis_env_vars.png "IIS Environment Variable Entries")

    As IIS deployment is handled in [Step 8b - Configure IIS with Leaf](../8b_configure_iis), you can wait to set environment variables until IIS configuration is complete.
---

<br>
The next step will depend on whether you are using Apache or IIS for deploying Leaf:

- [Step 8a - Configure Apache with Leaf](../8a_configure_apache), or
- [Step 8b - Configure IIS with Leaf](../8b_configure_iis)