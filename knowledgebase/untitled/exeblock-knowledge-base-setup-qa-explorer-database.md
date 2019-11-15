# EXEBlock Knowledge Base : Setup QA Explorer Database

* This guide will let you clone the existing development explorer database \( on 44 testnet as of now \) to QA environment \( 45 testnet\).
* Existing data on QA explorer database will be wiped out.
* Before doing this make sure with Alick that he does not need any data.
* mysql is already installed on both of these testnets.
*  on 44 testnet execute mysqldump -u root explorer -p &gt; full\_backup.sql \( root password will be prompted\)  → This will create the sqldump in a file named full\_backup\_sql \( Also its good to have a peek at whats inside the dump file\)
* Port the sqldump file from 44 testnet to 45 testnet. you can use the scp command.
* on 45 testnet execute mysql -u root -p explorer &lt;  full\_backup.sql \( root password will be prompted \) → This will execute all the instructions inside the dump file on the explorer database. This step might take longer time depending upon the number of records in the different tables.
* Its advisory to check the  process was successful and the tables exists      mysql -u root -p       use explorer;      show tables; Also for sanity you can randomly select 2 tables and check the record count match with development database. 

