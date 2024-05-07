# Installation Questions
#### Why do the App and Clinical databases need to be on the same server?
!!! tldr "Answer"
    **UPDATE 2022-05-05** - As of [Leaf v3.11](https://github.com/uwrit/leaf/releases/tag/v3.11.0), the App and Clinical databases **do not need to be on the same database server.**

    > Old answer (pre-v3.11): The reason for this is that after Leaf has executed a ‘Find Patients’ query, it caches the patient IDs in the app database. When users navigate to the ‘Visualize’ or ‘Patient List’ screens, Leaf performs a JOIN query between the cached patient IDs in the app database and patients in the clinical database to quickly retrieve patient demographics. Thus currently both databases need to be on the same server in order for the JOIN operation to work.

#### I'd like to use an RDBMS besides SQL Server. Does Leaf support any?
!!! tldr "Answer"
    As of [Leaf v3.11](https://github.com/uwrit/leaf/releases/tag/v3.11.0), Leaf does! For the Clinical DB, Leaf supports:

    - [MySQL](https://www.mysql.com/)
    - [MariaDB](https://mariadb.org/)
    - [PostgreSQL](https://www.postgresql.org/)
    - [Oracle](https://www.oracle.com/database/)
    - [Google BigQuery](https://cloud.google.com/bigquery/)
    - [SQL Server](https://www.microsoft.com/en-us/sql-server/default.aspx)

    **Note that the Leaf *App* DB must be deployed using SQL Server, however**.

#### Do you have Leaf installation videos?
!!! tldr "Answer"
      Links to installation videos:

    - [Leaf Overview](https://uwmedicine.mediasite.com/mediasite/Play/ff0cd75fbf7a4cab90928a1e406c31b11d)
    - [Installing Leaf](https://uwmedicine.mediasite.com/mediasite/Play/8ee792181ff74e06be90b41956f0aaa41d)
    - [Configuring Leaf](https://uwmedicine.mediasite.com/mediasite/Play/e18d3d9989574f5eba38ef09a69cc1391d)
