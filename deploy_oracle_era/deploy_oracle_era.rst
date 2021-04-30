.. _deploy_oracle_era:

-------------------------
Deploying Oracle with Era
-------------------------

In this lab, you use Era to deploy a new Oracle database.

Create Oracle Database with Era
+++++++++++++++++++++++++++++++

In this exercise you will deploy a new Oracle database using your *USERXX*\ **_ORACLE_19C** 1.0 Software Profile.

#. Select **Databases** from the drop-down menu, and then **Sources** from the left-hand menu.

#. Click **+ Provision > Oracle > Single Instance Database**.

#. In the **Provision an Oracle Single Instance Database** screen, fill out the following fields:

   - **Database Server** - Create New Server
   - **Database Server Name** - *USERXX*\ _oracle_prod
   - **Description** - (Optional)
   - **Nutanix Cluster** – EraCluster
   - **Software Profile** - *USERXX*\ _ORACLE_19C
   - **Compute Profile** - ORACLE_SMALL
   - **Network Profile** - Primary_ORACLE_NETWORK
   - **ASM Driver** - None (default)
   - Select **Enable High Availability (SIHA)**
   - **SYS ASM Password** - `Nutanix/4u`
   - **SSH Public Key for Node Access** - Select **Text**, and then click the icon in the upper right-hand corner of the window below to copy the script to your clipboard. You may then paste the following into the *SSH Public Key for Node Access* text box:

   .. code-block:: bash

         ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAii7qFDhVadLx5lULAG/ooCUTA/ATSmXbArs+GdHxbUWd/bNGZCXnaQ2L1mSVVGDxfTbSaTJ3En3tVlMtD2RjZPdhqWESCaoj2kXLYSiNDS9qz3SK6h822je/f9O9CzCTrw2XGhnDVwmNraUvO5wmQObCDthTXc72PcBOd6oa4ENsnuY9HtiETg29TZXgCYPFXipLBHSZYkBmGgccAeY9dq5ywiywBJLuoSovXkkRJk3cd7GyhCRIwYzqfdgSmiAMYgJLrz/UuLxatPqXts2D8v1xqR9EPNZNzgd4QHK4of1lqsNRuz2SxkwqLcXSw0mGcAL8mIwVpzhPzwmENC5Orw==


   .. note::

         By selecting Enable High Availability, Oracle Grid is configured as part of the deployment and Oracle Automatic Storage Management (ASM) is used for volume management. Without High Availability enabled, Linux LVM and file systems would be used for database storage. Grid and ASM are required for clustered Oracle RAC deployments.

   .. figure:: images/4.png

#. Click **Next**, and fill out the following fields:

   -  **Database Name** - *USERXX*\ _proddb
   -  **SID** - orclprod
   -  **Global Database Name** - orclprod (default)
   -  **SYS and SYSTEM Password** - `Nutanix/4u`
   -  **Database Parameter Profile** - ORACLE_SMALL_PARAMS

   .. figure:: images/5.png

   .. note::

      For each database engine supported by Era, you have the opportunity to run scripts before and/or after database creation. Common use cases include:

      - Data masking scripts.
      - Register the database with a database monitoring solution.
      - Scripts to update DNS/IPAM.
      - Scripts to automate application setup, such as app-level cloning for Oracle PeopleSoft.

      Encryption can be used in situations where compliance requires encryption and stops would-be attackers from bypassing the database and reading sensitive information directly from storage by enforcing data-at-rest encryption in the database layer.

#. Click **Next**, and then fill out the following fields to configure the Time Machine for your database:

   - **Name** - *USERXX*\ _proddb_TM (default)
   - **Description** - (Optional)
   - **SLA** - DEFAULT_OOB_GOLD_SLA
   - **Schedule** - (default)

   .. figure:: images/6.png

#. Click **Provision** to begin creating your new database server VM and *USERXX*\ **_proddb** database.

#. Select **Operations** from the drop-down menu to monitor the provisioning. This process should take approximately 30-60 minutes (depending on your cluster configuration). This must complete before you are able to proceed to the next section. Take a moment to stretch, take a bio break, and catch up on e-mail (you haven't been checking e-mail this whole time, have you?!).
