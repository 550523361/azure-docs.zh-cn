<properties
    pageTitle="管理弹性数据库池 (C#) | Azure"
    description="使用 C# 数据库开发技术来管理 Azure SQL 数据库弹性数据库池。"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.date="03/15/2016"
    wacn.date=""/>

# 管理弹性数据库池 (C&#x23;)

> [AZURE.SELECTOR]
- [Azure 门户](/documentation/articles/sql-database-elastic-pool-manage-portal)
- [PowerShell](/documentation/articles/sql-database-elastic-pool-manage-powershell)
- [C#](/documentation/articles/sql-database-elastic-pool-manage-csharp)


了解如何使用 C&#x23; 管理[弹性数据库池](/documentation/articles/sql-database-elastic-pool)。

> [AZURE.NOTE] 弹性数据库池目前为预览版，仅适用于 SQL 数据库 V12 服务器。如果你有一个 SQL 数据库 V11 服务器，可以通过一个步骤[使用 PowerShell 升级到 V12 并创建池](/documentation/articles/sql-database-upgrade-server-portal)。

这些示例使用[适用于 .NET 的 SQL 数据库库](https://msdn.microsoft.com/zh-cn/library/azure/mt349017.aspx)，因此你需要安装此库。你可以通过在 Visual Studio 中的[程序包管理器控制台](http://docs.nuget.org/Consume/Package-Manager-Console)中运行以下命令（“工具”>“NuGet 程序包管理器”>“程序包管理器控制台”）来进行安装：

    PM> Install-Package Microsoft.Azure.Management.Sql –Pre



## 更新弹性数据库池


    // Retrieve existing pool properties
    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);



## 将现有数据库移入弹性数据库池


    // Update database service objective to add the database to a pool

    // Retrieve current database properties
    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);




## 在弹性数据库池中创建新数据库


    // Create a new database in the pool

    // Create a database: configure create or update parameters and properties explicitly
    DatabaseCreateOrUpdateParameters newPooledDatabaseParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentServer.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = 268435456000, // 250 GB,
            Collation = "SQL_Latin1_General_CP1_CI_AS"
        }
    };

    var poolDbResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database2", newPooledDatabaseParameters);



## 列出弹性数据库池中的所有数据库

以下示例将列出池中的所有数据库：

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }




## 管理弹性数据库池 C&#x23; 示例

运行此示例需要以下库。你可以通过在 Visual Studio 中的[程序包管理器控制台](http://docs.nuget.org/Consume/Package-Manager-Console)中运行以下命令（“工具”>“NuGet 程序包管理器”>“程序包管理器控制台”）来进行安装

    PM> Install-Package Microsoft.Azure.Management.Sql –Pre
    PM> Install-Package Microsoft.Azure.Management.Resources –Pre
    PM> Install-Package Microsoft.Azure.Common.Authentication –Pre

创建控制台应用并将 Program.cs 的内容替换为以下内容。若要获取必需的客户端 id 和相关的值，请参阅[注册应用并获取所需的客户端值以便将你的应用连接到 SQL 数据库](/documentation/articles/sql-database-client-id-keys)。

    using Microsoft.Azure;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbElasticPoolsSample
    {
    class Program
    {
        // elastic database pool variables
        static string poolName = "elasticPool1";
        static string poolEdition = "Standard";
        static int poolDtus = 400;
        static int databaseMinDtus = 0;
        static int databaseMaxDtus = 100;

        // authentication variables
        static string subscriptionId = "<your Azure subscription id>";
        static string clientId = "<your client id>";
        static string redirectUri = "<your redirect URI>";
        static string domainName = "<i.e. microsoft.partner.onmschina.cn>";
        static AuthenticationResult token;

        // resource group variables
        static string datacenterLocation = "China East";
        static string resourceGroupName = "<resource group name>";

        // server variables
        static string serverName = "<server name>";
        static string adminLogin = "<server admin>";
        static string adminPassword = "<server admin password>";
        static string serverVersion = "12.0";

        // database variables
        static string databaseName = "<database name>";


        static void Main(string[] args)
        {
            // Sign in to Azure
            token = GetAccessToken();
            Console.WriteLine("Logged in as: " + token.UserInfo.DisplayableId);

            // Create a resource group
            ResourceGroup rg = CreateResourceGroup();
            Console.WriteLine(rg.Name + " created.");

            // Create a server
            Console.WriteLine("Creating server... ");
            ServerGetResponse srvr = CreateServer();
            Console.WriteLine("Creation of server " + srvr.Server.Name + ": " + srvr.StatusCode.ToString());

            // Create an elastic database pool
            Console.WriteLine("Creating elastic database pool with 400 pool eDTUs... ");
            ElasticPoolCreateOrUpdateResponse epool = CreateElasticDatabasePool();
            Console.WriteLine("Creation of pool " + epool.ElasticPool.Name + ": " + epool.Status.ToString());

            // Open the portal so we can see our operations in action
            string portalPage = @"https://manage.windowsazure.cn/#resource/subscriptions/"
                + subscriptionId
                + @"/resourceGroups/"
                + resourceGroupName
                + @"/providers/Microsoft.Sql/servers/"
                + serverName
                + @"/elasticPools/"
                + poolName ;
            System.Diagnostics.Process.Start(portalPage);


            // Pause the console until Enter is pressed.
            Console.WriteLine("Press Enter to update the pool to 1200 pool eDTUs.");
            Console.ReadLine();

            // Update the elastic database pool
            Console.WriteLine("Updating elastic database pool to 1200 pool eDTUs...");
            ElasticPoolCreateOrUpdateResponse epool2 = UpdateElasticDatabasePool();
            Console.WriteLine("Update of pool " + epool2.ElasticPool.Name + ": " + epool2.Status.ToString());

            // Create a new database in the pool
            Console.WriteLine("Creating a new database in the pool... ");
            DatabaseCreateOrUpdateResponse db = CreateNewDatabaseInPool();
            Console.WriteLine("Creation of elastic database " + db.Database.Name + ": " + db.Status.ToString());

            // Move an existing database into pool
            Console.WriteLine("Creating a new database, stand-alone, not yet in the pool... ");
            DatabaseCreateOrUpdateResponse db2 = CreateStandAloneDatabase();
            Console.WriteLine("Creation of stand-alone database " + db2.Database.Name + ": " + db2.Status.ToString());

            // Pause the console until Enter is pressed.
            Console.WriteLine("Press Enter to move our existing db into the pool.");
            Console.ReadLine();

            Console.WriteLine("Moving stand-alone database into the pool... ");
            DatabaseCreateOrUpdateResponse db3 = MoveExistingDatabaseIntoPool();
            Console.WriteLine("Moving stand-alone database " + db3.Database.Name + ": " + db3.Status.ToString());

            // Pause the console until Enter is pressed.
            Console.WriteLine("Press Enter to list all databases in {0}.", poolName);
            Console.ReadLine();

            // List all databases in a pool
            DatabaseListResponse dbs = GetDbsInPool();
            Console.WriteLine("Databases in Elastic Pool {0}", poolName);
            foreach (Database db4 in dbs)
            {
                Console.WriteLine("- " + db4.Name);
            }

            // Pause the console until Enter is pressed.
            Console.WriteLine("Press Enter to exit.");
            Console.ReadLine();
        }


        static ElasticPoolCreateOrUpdateResponse CreateElasticDatabasePool()
        {
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus, 
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, newPoolParameters);
            return newPoolResponse;
        }

        static ElasticPoolCreateOrUpdateResponse UpdateElasticDatabasePool()
        {
            int newPoolDtus = 1200;
            int newDatabaseMinDtus = 10;
            int newDatabaseMaxDtus = 50;

            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Retrieve existing pool
            var currentPool = sqlClient.ElasticPools.Get(resourceGroupName, serverName, poolName).ElasticPool;

            // Configure create or update parameters with existing property values, override those to be changed.
            ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = currentPool.Location,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = currentPool.Properties.Edition,
                    DatabaseDtuMax = newDatabaseMaxDtus,
                    DatabaseDtuMin = newDatabaseMinDtus,
                    Dtu = newPoolDtus
                }
            };
            var updatePoolResponse = sqlClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, updatePoolParameters);
            return updatePoolResponse;
        }

        static DatabaseCreateOrUpdateResponse CreateNewDatabaseInPool()
        {
            databaseName = "newDbInThePool";

            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Retrieve current pool
            var currentPool = sqlClient.ElasticPools.Get(resourceGroupName, serverName, poolName).ElasticPool;

            // Create a database
            DatabaseCreateOrUpdateParameters databaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentPool.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = "Default",
                    Edition = currentPool.Properties.Edition,
                    ElasticPoolName = poolName
                }
            };

            var dbResult = sqlClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, databaseParameters);
            return dbResult;
        }

        static DatabaseCreateOrUpdateResponse MoveExistingDatabaseIntoPool()
        {
            databaseName = "standaloneDB";

            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Retrieve current database
            var currentDatabase = sqlClient.Databases.Get(resourceGroupName, serverName, databaseName).Database;

            // Retrieve current pool
            var currentPool = sqlClient.ElasticPools.Get(resourceGroupName, serverName, poolName).ElasticPool;

            // Configure create or update parameters with existing property values, override those to be changed.
            DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentDatabase.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    Edition = currentPool.Properties.Edition,
                    RequestedServiceObjectiveName = "ElasticPool",
                    ElasticPoolName = currentPool.Name,
                    MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
                    Collation = currentDatabase.Properties.Collation,
                }
            };
            // Update the database
            var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, updatePooledDbParameters);
            return dbUpdateResponse;
        }

        static DatabaseListResponse GetDbsInPool()
        {
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            //List databases in the elastic pool
            DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases(resourceGroupName, serverName, poolName);
            return dbListInPool;
        }
     

        static ServerGetResponse CreateServer()
        {
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create a server
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = adminLogin,
                    AdministratorLoginPassword = adminPassword,
                    Version = serverVersion
                }
            };
            var serverResult = sqlClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }

        static DatabaseCreateOrUpdateResponse CreateStandAloneDatabase()
        {
            databaseName = "standaloneDB";
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create a database
            DatabaseCreateOrUpdateParameters databaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    Edition = "Standard",
                    RequestedServiceObjectiveName = "S0",
                    CreateMode = "Default"
                }
            };

            var dbResult = sqlClient.Databases.CreateOrUpdate(resourceGroupName,serverName, databaseName, databaseParameters);
            return dbResult;
        }

        static ResourceGroup CreateResourceGroup()
        {
            Microsoft.Rest.TokenCredentials creds = new Microsoft.Rest.TokenCredentials(token.AccessToken);
            // Create a resource management client 
            ResourceManagementClient resourceClient = new ResourceManagementClient(creds);

            // Resource group parameters
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = datacenterLocation,
            };

            //Create a resource group
            resourceClient.SubscriptionId = subscriptionId;
            var resourceGroupResult = resourceClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        private static AuthenticationResult GetAccessToken()
        {
            AuthenticationContext authContext = new AuthenticationContext
                ("https://login.chinacloudapi.cn/" + domainName /* Tenant ID or AAD domain */);

            AuthenticationResult token = authContext.AcquireToken
                ("https://management.azure.com/"/* the Azure Resource Management endpoint */,
                    clientId,
            new Uri(redirectUri) /* redirect URI */,
            PromptBehavior.Auto /* with Auto user will not be prompted if an unexpired token is cached */);

            return token;
        }
    }
    }



## 其他资源


- [SQL 数据库](/documentation/services/sql-databases)
- [Azure 资源管理 API](https://msdn.microsoft.com/zh-cn/library/azure/dn948464.aspx)
- [弹性数据库池参考](/documentation/articles/sql-database-elastic-pool-reference)

<!---HONumber=Mooncake_0328_2016-->