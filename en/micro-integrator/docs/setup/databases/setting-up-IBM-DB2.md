# Setting up IBM DB2

The following sections describe how to set up an IBM DB2 database to replace the default H2 database in your WSO2 product:

## Prerequisites

Download the latest version of [DB2
Express-C](http://www-01.ibm.com/software/data/db2/express/download.html)
and install it on your computer.

> For instructions on installing DB2 Express-C, see this [ebook](https://www.ibm.com/developerworks/community/wikis/home?lang=en#!/wiki/Big%20Data%20University/page/FREE%20eBook%20-%20Getting%20Started%20with%20DB2%20Express-C).

## Setting up the database and users

Create the database using either [DB2 command processor](#SettingupIBMDB2-UsingtheDB2commandprocessor) or [DB2 control
center](#SettingupIBMDB2-UsingtheDB2controlcenter) as described below.

### Using the DB2 command processor

1.  Run DB2 console and execute the `          db2start         `
    command on a CLI to open DB2.
2.  Create the database using the following command:  
    `          create database <DB_NAME>         `
3.  Before issuing an SQL statement, establish the connection to the
    database using the following command:  
    `          connect to <DB_NAME> user <USER_ID> using <PASSWORD>         `
4.  Grant required permissions for users as follows:

    ``` actionscript3
        connect to DB_NAME
        grant <AUTHORITY> on database to user <USER_ID>
    ```

    For example:

    ![](attachments/53125504/53287380.png){width="550"}

     > For more information on DB2 commands, see the [DB2 Express-C Guide](https://www.ibm.com/developerworks/community/wikis/home?lang=en#!/wiki/Big%20Data%20University/page/FREE%20eBook%20-%20Getting%20Started%20with%20DB2%20Express-C).
    
### Using the DB2 control center

1.  Open the DB2 control center using the `          db2cc         `
    command as follows:  

    ![](attachments/53125504/53287383.png)

2.  Right-click **All Databases** in the control center tree (inside the
    object browser), click **Create Database** , and then click
    **Standard** and follow the steps in the **Create New Database**
    wizard.  
    ![](attachments/53125504/53287398.png){width="580"}
3.  Click **User and Group Objects** in the control center tree to
    create users for the newly created database.  
    ![](attachments/53125504/53287381.png){width="780"}
4.  Give the required permissions to the newly created users.  
    ![](attachments/53125504/53287382.png){width="420"}

## Setting up DB2 JDBC drivers

Copy the DB2 JDBC drivers ( `         db2jcc.jar        ` and
`         db2jcc_license_c0u.jar        ` ) from
`         <DB2_HOME>/SQLLIB/java/        ` directory to the
`         <PRODUCT_HOME>/repository/components/lib/        ` directory.

![](attachments/53125504/53287393.png)

> `         <DB2_HOME>        ` refers to the installation directory of
DB2 Express-C, and \< `         PRODUCT        `
`         _HOME>        ` refers to the directory where you run the WSO2
product instance.


## Connecting the database to the server

To enable the two nodes to access the shared database, update the following parameters in the esb.toml file.

``` Java
// The config section that groups the parameters for the primary database that will be shared by both product nodes in the cluster.
[database.shared.db]

// Specify the type of database.
type = "ibm_db"

// Specify the connection URL of your database. The following default URL connects to the H2 database that is shipped with the product.
url="jdbc:db2://SERVER_NAME:PORT/DB_NAME"

// The username for connecting to the database. By default, 'root' is the MySQL username.
username = "regadmin"

// The password for connecting to the database. By default, 'root' is the MySQL password.
password = "regadmin"

```

Find more parameters for [connecting to the primary database](../../../references/ei_config_catalog/#connecting-to-the-primary-data-store) and for 
[tuning the primary database](../../../references/ei_config_catalog/#tuning-the-primary-data-store-connection).