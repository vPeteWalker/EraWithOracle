.. _admin_oracle:

--------------------------------
Database Administration with Era
--------------------------------

We will now see how to perform common database administration tasks with Era.

In this lab, you will administer your Oracle database.

Explore Your Database
+++++++++++++++++++++

#. In **Era**, select **Databases** from the drop-down menu, and then **Sources** from the left-hand menu.

   .. figure:: images/1.png

#. Click on *USERXX*\ **_proddb**. This page provides details of the database, database server VM access, Time Machine schedule, and Compute/Network/Software profiles used to provision.

    .. figure:: images/2.png

Snapshot Your Database
++++++++++++++++++++++

Before we create a manual snapshot of your database, let's create a new table.

Create New Table
................

#. Within **Prism Central > Virtual Infrastructure > VMs > List**, identify the IP address assigned to the *USERXX*\ **_oracle_prod** VM using the *IP Addresses* column.

#. SSH (Terminal/Putty) into your *USERXX*\ **_oracle_prod** VM.

   - **User Name** - oracle
   - **Password** - Nutanix/4u

   .. code-block:: Bash

     ssh oracle@<USERXX_ORACLE_PROD_CLONE1-IP-ADDRESS>

#. Launch **sqlplus**

     .. code-block:: Bash

       sqlplus / as sysdba

#. Execute the following to create a table:

     .. code-block:: Bash

       CREATE TABLE testlabtable
       (
       column1 VARCHAR(30),
       column2 DATE
       );

#. Verify the new table is there by executing the following to list the table:

     .. code-block:: Bash

       select owner as schema_name,
       table_name
       from sys.all_tables
       where table_name like 'TEST%';

   Here is a sample output of the above commands.

      .. figure:: images/5.png

#. Exit your Terminal or Putty session.

Create Manual Snapshot
......................

#. Within **Era**, select **Databases** from the drop-down menu, and then **Sources** from the left-hand menu.

#. Click on the Time Machine for your database: *USERXX*\ **_proddb_TM**

   .. figure:: images/6.png

#. Click **Actions > Log Catch Up**.

   .. figure:: images/12.png

#. Click **Yes** on the *Confirmation* dialogue box.

   .. figure:: images/13.png

   .. note::

      The time shown for log catch up is an example. Your time may vary. Regardless, you will proceed by clicking **Yes**.

#. Select **Operations** from the drop-down menu to monitor the progress. This process should take approximately 2-5 minutes. Please wait for the *Log Catch Up* operation to successfully complete before moving on to the next step.

#. Return to **Databases > Sources > Time Machine for your database**: *USERXX*\ **_proddb_TM**

#. Click **Actions > Snapshot**.

   .. figure:: images/7.png

#. In the Create Snapshot wizard, complete the following:
   - **Snapshot Name** - *USERXX*\ _proddb_1st_snapshot

   .. figure:: images/8.png

#. Click **Create**.

#. Select **Operations** from the drop-down menu to monitor the progress. This process should take approximately 2-5 minutes. Please wait for the *Create Snapshot* operation to successfully complete before moving on to the next step.

Clone Your Database Server & Database
+++++++++++++++++++++++++++++++++++++

#. Within **Era**, select **Time Machines** from the drop-down menu, and then click on *USERXX*\ **_proddb_TM**.

#. Click **Actions > Create Single Instance Database Clone**.

   - **Clone a Snapshot**
   - Select the newly created snapshot - *USERXX*\ _proddb_1st_snapshot (Date Time)

   .. figure:: images/9.png

#. Click **Next**

   - **Database Server VM** - Create New Server
   - **Database Server VM Name** - *USERXX*\ _oracle_prod_clone1
   - **Compute Profile** - ORACLE_SMALL
   - **Network Profile** - Primary_ORACLE_Network
   - **SSH Public Key Through** - Select **Text**
   - Copy the following text and paste it into the **SSH Public Key** text box:

   .. code-block:: bash

      ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAii7qFDhVadLx5lULAG/ooCUTA/ATSmXbArs+GdHxbUWd/bNGZCXnaQ2L1mSVVGDxfTbSaTJ3En3tVlMtD2RjZPdhqWESCaoj2kXLYSiNDS9qz3SK6h822je/f9O9CzCTrw2XGhnDVwmNraUvO5wmQObCDthTXc72PcBOd6oa4ENsnuY9HtiETg29TZXgCYPFXipLBHSZYkBmGgccAeY9dq5ywiywBJLuoSovXkkRJk3cd7GyhCRIwYzqfdgSmiAMYgJLrz/UuLxatPqXts2D8v1xqR9EPNZNzgd4QHK4of1lqsNRuz2SxkwqLcXSw0mGcAL8mIwVpzhPzwmENC5Orw==

   .. figure:: images/10.png

#. Click **Next**

   - **Name** - *USERXX*\ _proddb_clone1
   -  **SID** - orclprod (default)
   -  **SYS and SYSTEM Password** - `Nutanix/4u`
   -  **Database Parameter Profile** - ORACLE_SMALL_PARAMS

   .. figure:: images/11.png

#. Click **Clone**.

#. Select **Operations** from the drop-down menu to monitor the progress. This process should take approximately 20-40 minutes. Please wait for the *Clone Database* operation to successfully complete before moving on to the next step.

Delete Table and Clone Refresh
++++++++++++++++++++++++++++++

There are times when a table or other data gets deleted, and you would like to get it back. Here we will delete a table, and use the Era *Clone Refresh* action from the last snapshot we created to restore that table.

Delete Table
............

#. Within **Prism Central > Virtual Infrastructure > VMs > List**, identify the IP address assigned to the *USERXX*\ **_oracle_prod_clone1** VM using the *IP Addresses* column.

#. SSH (Terminal/Putty) into your *USERXX*\ **_oracle_prod_clone1** VM.

   - **User Name** - oracle
   - **Password** - Nutanix/4u

   .. code-block:: bash

      ssh oracle@<USERXX_ORACLE_PROD_CLONE1-IP-ADDRESS>

#. Launch *sqlplus*.

   .. code-block:: bash

      sqlplus / as sysdba

#. Execute the following to drop the table:

   .. code-block:: bash

      DROP TABLE testlabtable;

#. Verify the table has been removed by executing the following to list the table:

   .. code-block:: bash

      select owner as schema_name,
      table_name
      from sys.all_tables
      where table_name like 'TEST%';

   Sample output

   .. figure:: images/14.png

#.	leave your Terminal or Putty session active, as we will be returning to it shortly.

Refresh Clone
.............

#. In **Era**, select **Databases** from the drop-down menu, and then **Clones** from the left-hand menu.

#. Select the Clone for your database *USERXX*\ proddb_clone1, and then click **Refresh**.

   - **Refresh to a** - Snapshot
   - Select the snapshot - *USERXX*\ _proddb_1st_snapshot (Date Time)

#. Click **Refresh**.

#. Select **Operations** from the drop-down menu to monitor the registration. This process should take approximately 10-15 minutes. Please wait for the *Refresh Clone* operation to successfully complete before moving on to the next step.

Verify Table Has Been Restored
..............................

#. SSH (Terminal/Putty) into your *USERXX*\ **_oracle_prod_clone1** VM

   - **User Name** - oracle
   - **Password** - Nutanix/4u

   .. code-block:: Bash

     ssh oracle@<USERXX_ORACLE_PROD_CLONE1-IP-ADDRESS>

#. Launch **sqlplus**

     .. code-block:: Bash

       sqlplus / as sysdba

#. Verify the table is back by executing the following to list the table:

     .. code-block:: bash

       select owner as schema_name,
       table_name
       from sys.all_tables
       where table_name like 'TEST%';

   Sample output

   .. figure:: images/15.png
