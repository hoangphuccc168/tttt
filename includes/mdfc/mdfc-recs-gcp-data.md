---
author: elazark
ms.service: defender-for-cloud
ms.topic: include
ms.date: 11/09/2022
ms.author: elkrieger
ms.custom: generated
---

There are **28** GCP recommendations in this category.

|Recommendation |Description |Severity |
|---|---|---|
|[Ensure '3625 (trace flag)' database flag for Cloud SQL SQL Server instance is set to 'off'](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/631246fb-7192-4709-a0b3-b83e65e6b550) |It is recommended to set "3625 (trace flag)" database flag for Cloud SQL SQL Server instance to "off".<br> Trace flags are frequently used to diagnose performance issues or to debug stored procedures or complex computer systems, but they may also be recommended by Microsoft Support to address behavior that is negatively impacting a specific workload.<br> All documented trace flags and those recommended by Microsoft Support are fully supported in a production environment when used as directed.<br> "3625(trace log)" Limits the amount of information returned to users who are not members of the sysadmin fixed server role, by masking the parameters of some error messages using '******'.<br> This can help prevent disclosure of sensitive information, hence this is recommended to disable this flag.<br> This recommendation is applicable to SQL Server database instances. |Medium |
|[Ensure 'external scripts enabled' database flag for Cloud SQL SQL Server instance is set to 'off'](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/98b8908a-18b9-46ea-8c52-3f8db1da996f) |It is recommended to set "external scripts enabled" database flag for Cloud SQL SQL Server instance to off.<br> "external scripts enabled" enable the execution of scripts with certain remote language extensions.<br> This property is OFF by default.<br> When Advanced Analytics Services is installed, setup can optionally set this property to true.<br> As the "External Scripts Enabled" feature allows scripts external to SQL such as files located in an R library to be executed, which could adversely affect the security of the system, hence this should be disabled.<br> This recommendation is applicable to SQL Server database instances. |High |
|[Ensure 'remote access' database flag for Cloud SQL SQL Server instance is set to 'off'](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/dddbbe7d-7e32-47d8-b319-39cbb70b8f88) |It is recommended to set "remote access" database flag for Cloud SQL SQL Server instance to "off".<br> The "remote access" option controls the execution of stored procedures from local or remote servers on which instances of SQL Server are running.<br> This default value for this option is 1.<br> This grants permission to run local stored procedures from remote servers or remote stored procedures from the local server.<br> To prevent local stored procedures from being run from a remote server or remote stored procedures from being run on the local server, this must be disabled.<br> The Remote Access option controls the execution of local stored procedures on remote servers or remote stored procedures on local server.<br> 'Remote access' functionality can be abused to launch a Denial-of-Service (DoS) attack on remote servers by off-loading query processing to a target, hence this should be disabled.<br> This recommendation is applicable to SQL Server database instances. |High |
|[Ensure 'skip_show_database' database flag for Cloud SQL Mysql instance is set to 'on'](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/9e5b33de-bcfa-4044-93ce-4937bf8f0bbd) |It is recommended to set "skip_show_database" database flag for Cloud SQL Mysql instance to "on".<br> 'skip_show_database' database flag prevents people from using the SHOW DATABASES statement if they do not have the SHOW DATABASES privilege.<br> This can improve security if you have concerns about users being able to see databases belonging to other users.<br> Its effect depends on the SHOW DATABASES privilege: If the variable value is ON, the SHOW DATABASES statement is permitted only to users who have the SHOW DATABASES privilege, and the statement displays all database names.<br> If the value is OFF, SHOW DATABASES is permitted to all users, but displays the names of only those databases for which the user has the SHOW DATABASES or other privilege.<br> This recommendation is applicable to Mysql database instances.<br>  |Low |
|[Ensure that a Default Customer-managed encryption key (CMEK) is specified for all BigQuery Data Sets](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/f024ea22-7e48-4b3b-a824-d61794c14bb4) |BigQuery by default encrypts the data as rest by employing Envelope Encryption using Google managed cryptographic keys.<br> The data is encrypted using the data encryption keys and data encryption keys themselves are further encrypted using key encryption keys. <br>This is seamless and do not require any additional input from the user. <br>However, if you want to have greater control, Customer-managed encryption keys (CMEK) can be used as encryption key management solution for BigQuery Data Sets.<br>BigQuery by default encrypts the data as rest by employing Envelope Encryption using Google managed cryptographic keys.<br> This is seamless and does not require any additional input from the user.<br>For greater control over the encryption, customer-managed encryption keys (CMEK) can be used as encryption key management solution for BigQuery Data Sets.<br> Setting a Default Customer-managed encryption key (CMEK) for a data set ensure any tables created in future will use the specified CMEK if none other is provided.<br>Note: Google does not store your keys on its servers and cannot access your protected data unless you provide the key. <br>This also means that if you forget or lose your key, there is no way for Google to recover the key or to recover any data encrypted with the lost key. |Medium |
|[Ensure that all BigQuery Tables are encrypted with Customer-managed encryption key (CMEK)](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/f4cfc689-cac8-4f45-8355-652dcda3ec55) |BigQuery by default encrypts the data as rest by employing Envelope Encryption using Google managed cryptographic keys. <br> The data is encrypted using the data encryption keys and data encryption keys themselves are further encrypted using key encryption keys.<br> This is seamless and do not require any additional input from the user.<br> However, if you want to have greater control, Customer-managed encryption keys (CMEK) can be used as encryption key management solution for BigQuery Data Sets.<br> If CMEK is used, the CMEK is used to encrypt the data encryption keys instead of using google-managed encryption keys.<br>BigQuery by default encrypts the data as rest by employing Envelope Encryption using Google managed cryptographic keys. <br>This is seamless and does not require any additional input from the user.<br>For greater control over the encryption, customer-managed encryption keys (CMEK) can be used as encryption key management solution for BigQuery tables. <br>The CMEK is used to encrypt the data encryption keys instead of using google-managed encryption keys.<br> BigQuery stores the table and CMEK association and the encryption/decryption is done automatically.<br>Applying the Default Customer-managed keys on BigQuery data sets ensures that all the new tables created in the future will be encrypted using CMEK but existing tables need to be updated to use CMEK individually. <br>Note: Google does not store your keys on its servers and cannot access your protected data unless you provide the key.<br> This also means that if you forget or lose your key, there is no way for Google to recover the key or to recover any data encrypted with the lost key. |Medium |
|[Ensure that BigQuery datasets are not anonymously or publicly accessible](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/dab1eea3-7693-4da3-af1b-2f73832655fa) |It is recommended that the IAM policy on BigQuery datasets does not allow anonymous and/or public access.<br/>  Granting permissions to allUsers or allAuthenticatedUsers allows anyone to access the dataset. <br/>Such access might not be desirable if sensitive data is being stored in the dataset.<br/> Therefore, ensure that anonymous and/or public access to a dataset is not allowed. |High |
|[Ensure that Cloud SQL database instances are configured with automated backups](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/afaac6e6-6240-48a2-9f62-4e257b851311) |It is recommended to have all SQL database instances set to enable automated backups. <br> Backups provide a way to restore a Cloud SQL instance to recover lost data or recover from a problem with that instance. <br> Automated backups need to be set for any instance that contains data that should be protected from loss or damage. <br> This recommendation is applicable for SQL Server, PostgreSql, MySql generation 1 and MySql generation 2 instances. |High |
|[Ensure that Cloud SQL database instances are not open to the world](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/de78ebca-1ec6-4872-8061-8fcfb27752fc) |Database Server should accept connections only from trusted Network(s)/IP(s) and restrict access from the world. <br> To minimize attack surface on a Database server instance, only trusted/known and required IP(s) should be white-listed to connect to it. <br> An authorized network should not have IPs/networks configured to "0.0.0.0/0" which will allow access to the instance from anywhere in the world. Note that authorized networks apply only to instances with public IPs. |High |
|[Ensure that Cloud SQL database instances do not have public IPs](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/1658239d-caf7-471d-83c5-2e4c44afdcff) |It is recommended to configure Second Generation Sql instance to use private IPs instead of public IPs. <br> To lower the organization's attack surface, Cloud SQL databases should not have public IPs. <br> Private IPs provide improved network security and lower latency for your application. |High |
|[Ensure that Cloud Storage bucket is not anonymously or publicly accessible](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/d8305d96-2aa5-458d-92b7-f8418f5f3328) |It is recommended that IAM policy on Cloud Storage bucket does not allows anonymous or public access.<br/>Allowing anonymous or public access grants permissions to anyone to access bucket content. <br/> Such access might not be desired if you are storing any sensitive data. <br/> Hence, ensure that anonymous or public access to a bucket is not allowed. |High |
|[Ensure that Cloud Storage buckets have uniform bucket-level access enabled](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/64b5cdbc-0633-49af-b63d-a9dc90560196) |It is recommended that uniform bucket-level access is enabled on Cloud Storage buckets. <br/> It is recommended to use uniform bucket-level access to unify and simplify how you grant access to your Cloud Storage resources.<br/> Cloud Storage offers two systems for granting users permission to access your buckets and objects: <br/> Cloud Identity and Access Management (Cloud IAM) and Access Control Lists (ACLs).  <br/> These systems act in parallel - in order for a user to access a Cloud Storage resource, only one of the systems needs to grant the user permission. <br/> Cloud IAM is used throughout Google Cloud and allows you to grant a variety of permissions at the bucket and project levels.<br/> ACLs are used only by Cloud Storage and have limited permission options, but they allow you to grant permissions on a per-object basis. <br/><br/> In order to support a uniform permissioning system, Cloud Storage has uniform bucket-level access.<br/> Using this feature disables ACLs for all Cloud Storage resources: <br/> access to Cloud Storage resources then is granted exclusively through Cloud IAM. <br/> Enabling uniform bucket-level access guarantees that if a Storage bucket is not publicly accessible, <br/>no object in the bucket is publicly accessible either. |Medium |
|[Ensure that Compute instances have Confidential Computing enabled](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/171e9492-73a7-43de-adce-6bd0a3cf6045) |Google Cloud encrypts data at-rest and in-transit, but customer data must be decrypted for processing. Confidential Computing is a breakthrough technology which encrypts data in-use-while it is being processed.<br> Confidential Computing environments keep data encrypted in memory and elsewhere outside the central processing unit (CPU). <br>Confidential VMs leverage the Secure Encrypted Virtualization (SEV) feature of AMD EPYC CPUs.<br> Customer data will stay encrypted while it is used, indexed, queried, or trained on.<br> Encryption keys are generated in hardware, per VM, and not exportable. Thanks to built-in hardware optimizations of both performance and security, there is no significant performance penalty to Confidential Computing workloads.<br>Confidential Computing enables customers' sensitive code and other data encrypted in memory during processing. Google does not have access to the encryption keys. <br>Confidential VM can help alleviate concerns about risk related to either dependency on Google infrastructure or Google insiders' access to customer data in the clear. |High |
|[Ensure that retention policies on log buckets are configured using Bucket Lock](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/07ca1398-d477-400a-a9fc-4cfc78f723f9) |Enabling retention policies on log buckets will protect logs stored in cloud storage buckets from being overwritten or accidentally deleted. <br> It is recommended to set up retention policies and configure Bucket Lock on all storage buckets that are used as log sinks. <br> Logs can be exported by creating one or more sinks that include a log filter and a destination. As Stackdriver Logging receives new log entries, they are compared against each sink. <br> If a log entry matches a sink's filter, then a copy of the log entry is written to the destination. <br> Sinks can be configured to export logs in storage buckets. <br> It is recommended to configure a data retention policy for these cloud storage buckets and to lock the data retention policy; thus permanently preventing the policy from being reduced or removed. <br> This way, if the system is ever compromised by an attacker or a malicious insider who wants to cover their tracks, the activity logs are definitely preserved for forensics and security investigations. |Low |
|[Ensure that the Cloud SQL database instance requires all incoming connections to use SSL](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/13872d43-aac6-4018-9c89-507b8fe9be54) |It is recommended to enforce all incoming connections to SQL database instance to use SSL. <br> SQL database connections if successfully trapped (MITM); can reveal sensitive data like credentials, database queries, query outputs etc. <br> For security, it is recommended to always use SSL encryption when connecting to your instance. <br> This recommendation is applicable for Postgresql, MySql generation 1 and MySql generation 2 instances. |High |
|[Ensure that the 'contained database authentication' database flag for Cloud SQL on the SQL Server instance is set to 'off'](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/658ce98f-ecf1-4c14-967f-3c4faf130fbf) |It is recommended to set "contained database authentication" database flag for Cloud SQL on the SQL Server instance is set to "off". <br> A contained database includes all database settings and metadata required to define the database and has no configuration dependencies on the instance of the Database Engine where the database is installed. <br> Users can connect to the database without authenticating a login at the Database Engine level. <br> Isolating the database from the Database Engine makes it possible to easily move the database to another instance of SQL Server. <br> Contained databases have some unique threats that should be understood and mitigated by SQL Server Database Engine administrators. <br> Most of the threats are related to the USER WITH PASSWORD authentication process, which moves the authentication boundary from the Database Engine level to the database level, hence this is recommended to disable this flag. <br> This recommendation is applicable to SQL Server database instances. |Medium |
|[Ensure that the 'cross db ownership chaining' database flag for Cloud SQL SQL Server instance is set to 'off'](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/26973a34-79a6-46a0-874f-358c8c00af05) |It is recommended to set "cross db ownership chaining" database flag for Cloud SQL SQL Server instance to "off". <br> Use the "cross db ownership" for chaining option to configure cross-database ownership chaining for an instance of Microsoft SQL Server. <br> This server option allows you to control cross-database ownership chaining at the database level or to allow cross-database ownership chaining for all databases. <br> Enabling "cross db ownership" is not recommended unless all of the databases hosted by the instance of SQL Server must participate in cross-database ownership chaining and you are aware of the security implications of this setting. <br> This recommendation is applicable to SQL Server database instances. |Medium |
|[Ensure that the 'local_infile' database flag for a Cloud SQL Mysql instance is set to 'off'](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/633a87f4-bd71-45ce-9eca-c6bb8cbe8b21) |It is recommended to set the local_infile database flag for a Cloud SQL MySQL instance to off.<br />The local_infile flag controls the server-side LOCAL capability for LOAD DATA statements. Depending on the local_infile setting, the server refuses or permits local data loading by clients that have LOCAL enabled on the client side.<br />To explicitly cause the server to refuse LOAD DATA LOCAL statements (regardless of how client programs and libraries are configured at build time or runtime), start mysqld with local_infile disabled. local_infile can also be set at runtime.<br />Due to security issues associated with the local_infile flag, it is recommended to disable it. This recommendation is applicable to MySQL database instances. |Medium |
|[Ensure that the log metric filter and alerts exist for Cloud Storage IAM permission changes](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/2e14266c-76ea-4479-915e-4edaae7d78ec) |It is recommended that a metric filter and alarm be established for Cloud Storage Bucket IAM changes. <br>Monitoring changes to cloud storage bucket permissions may reduce the time needed to detect and correct permissions on sensitive cloud storage buckets and objects inside the bucket. |Low |
|[Ensure that the log metric filter and alerts exist for SQL instance configuration changes](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/9dce022e-f7f9-4725-8a63-c0d4a868b4d3) |It is recommended that a metric filter and alarm be established for SQL instance configuration changes. <br>Monitoring changes to SQL instance configuration changes may reduce the time needed to detect and correct misconfigurations done on the SQL server. <br>Below are a few of the configurable options which may the impact security posture of an SQL instance: <br>* Enable auto backups and high availability: Misconfiguration may adversely impact business continuity, disaster recovery, and high availability <br>* Authorize networks: Misconfiguration may increase exposure to untrusted networks |Low |
|[Ensure that there are only GCP-managed service account keys for each service account](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/6991b2e9-ae9e-4e99-acb6-037c4b575215) |User managed service accounts should not have user-managed keys.<br> Anyone who has access to the keys will be able to access resources through the service account. GCP-managed keys are used by Cloud Platform services such as App Engine and Compute Engine. These keys cannot be downloaded. Google will keep the keys and automatically rotate them on an approximately weekly basis.<br> User-managed keys are created, downloadable, and managed by users. They expire 10 years from creation.<br>For user-managed keys, the user has to take ownership of key management activities which include:<ul> <li> Key storage</li><li>Key distribution</li><li>Key revocation </li><li>Key rotation</li><li>Protecting the keys from unauthorized users</li><li>Key recovery</li></ul>Even with key owner precautions, keys can be easily leaked by common development malpractices like checking keys into the source code or leaving them in the Downloads directory, or accidentally leaving them on support blogs/channels. It is recommended to prevent user-managed service account keys. |Low |
|[Ensure 'user connections' database flag for Cloud SQL SQL Server instance is set as appropriate](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/91f55b07-083c-4ec5-a2be-4b52bbc2e2df) |It is recommended to set "user connections" database flag for Cloud SQL SQL Server instance according organization-defined value.<br> The "user connections" option specifies the maximum number of simultaneous user connections that are allowed on an instance of SQL Server.<br> The actual number of user connections allowed also depends on the version of SQL Server that you are using, and also the limits of your application or applications and hardware.<br> SQL Server allows a maximum of 32,767 user connections.<br> Because user connections is a dynamic (self-configuring) option, SQL Server adjusts the maximum number of user connections automatically as needed, up to the maximum value allowable.<br> For example, if only 10 users are logged in, 10 user connection objects are allocated.<br> In most cases, you do not have to change the value for this option.<br> The default is 0, which means that the maximum (32,767) user connections are allowed.<br> This recommendation is applicable to SQL Server database instances. |Low |
|[Ensure 'user options' database flag for Cloud SQL SQL Server instance is not configured](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/fab1e680-86f0-4616-bee9-1b7394e49ade) |It is recommended that, "user options" database flag for Cloud SQL SQL Server instance should not be configured.<br> The "user options" option specifies global defaults for all users.<br> A list of default query processing options is established for the duration of a user's work session.<br> The user options option allows you to change the default values of the SET options (if the server's default settings are not appropriate).<br> A user can override these defaults by using the SET statement.<br> You can configure user options dynamically for new logins.<br> After you change the setting of user options, new login sessions use the new setting; current login sessions are not affected.<br> This recommendation is applicable to SQL Server database instances. |Low |
|[Logging for GKE clusters should be enabled](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/fa160a2c-e976-41cb-acff-1e1e3f1ed032) |This recommendation evaluates whether the loggingService property of a cluster contains the location Cloud Logging should use to write logs. |High |
|[Object versioning should be enabled on storage buckets where sinks are configured](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/e836b239-c7dc-476a-9a85-829b565cbc59) |This recommendation evaluates whether the enabled field in the bucket's versioning property is set to true. |High |
|[Over-provisioned identities in projects should be investigated to reduce the Permission Creep Index (PCI)](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/a6cd9b98-3b29-4213-b880-43f0b0897b83) |Over-provisioned identities in projects should be investigated to reduce the Permission Creep Index (PCI) and to safeguard your infrastructure. Reduce the PCI by removing the unused high risk permission assignments. High PCI reflects risk associated with the identities with permissions that exceed their normal or required usage |Medium |
|[Projects that have cryptographic keys should not have users with Owner permissions](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/986fe72e-466a-462d-a06e-c77b439c53c0) |This recommendation evaluates the IAM allow policy in project metadata for principals assigned roles/Owner. |Medium |
|[Storage buckets used as a log sink should not be publicly accessible](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/76261631-76ea-4bd4-b064-34a619be1de0) |This recommendation evaluates the IAM policy of a bucket for the principals allUsers or allAuthenticatedUsers, which grant public access. |High |
|||