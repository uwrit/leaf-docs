# Installation Questions
#### Why do the App and Clinical databases [need to be on the same server](../installation/installation_steps/1_app_db.md)?
!!! tldr "Answer"
    The reason for this is that after Leaf has executed a ‘Find Patients’ query, it [caches the patient IDs in the app database](https://github.com/uwrit/leaf/blob/master/src/server/Services/Cohort/CohortCacheService.cs#L58). When users navigate to the ‘Visualize’ or ‘Patient List’ screens, Leaf [performs a JOIN query](https://github.com/uwrit/leaf/blob/master/src/server/Model/Compiler/SqlServer/DemographicSqlCompiler.cs#L80) between the cached patient IDs in the app database and patients in the clinical database to quickly retrieve patient demographics. Thus currently both databases need to be on the same server in order for the JOIN operation to work.

#### I'd like to use an RDBMS besides SQL Server. Does Leaf support any?
!!! tldr "Answer"
    We understand - there are many great RDBMSs besides SQL Server (MySQL/MariaDB, PostgreSQL, Oracle, BigQuery, etc.), and being restricted to SQL Server can be a pain. While Leaf *currently* only supports SQL Server, we are actively evaluating methods for allowing Leaf to query non-SQL Server databases as well.

    So ***if***, in a future release we are able to have Leaf work with other RDBMSs, please note that it will likely **only be the clinical database** (as opposed to the **app** database), as the work involved in making the clinical database more flexible is much simpler than the app database. (Because the app database has various stored procedures and functions which would need to be written in other SQL dialects, while the clinical database simply executes standard SQL statements).

    In other words, in the future we ***may*** support other *clinical* database flavors, but **you would still need to run the Leaf *application* database in SQL Server**.