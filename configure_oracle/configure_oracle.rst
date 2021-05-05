.. _configure_oracle:

-------------------------------------------
Configuring Your Oracle and Era Environment
-------------------------------------------

Traditional database VM deployment resembles the diagram below. The process generally starts with a IT ticket for a database (from Dev, Test, QA, Analytics, etc.). Next one or more teams will need to deploy the storage resources and VM(s) required. Once infrastructure is ready, a DBA needs to provision and configure database software. Once provisioned, any best practices and data protection/backup policies need to be applied. Finally the database can be handed over to the end user. That's a lot of handoffs, and the potential for a lot of friction.

.. figure:: images/0.png

Whereas with a Nutanix cluster and Era, provisioning and protecting a database should take you no longer than it took to read this intro.

   .. note::

      During this workshop, if you receive any errors when inputting information, that may be a result of copying and pasting into that particular screen. Please attempt to manually type in the same information, and proceed

Exploring Era Resources
+++++++++++++++++++++++

Era is distributed as a virtual appliance that can be installed on either AHV or ESXi. For the purposes of this workshop, a shared Era server has already been deployed on your cluster.

.. note::

   If you're interested, instructions for the brief installation of the Era appliance can be found `here <https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Era-User-Guide-v2_1:era-era-installation-c.html>`_. This includes instructions for both AHV and ESXi.

#. In **Prism Central > VMs > List**, identify the IP address assigned to the **EraServer-\*** VM using the **IP Addresses** column.

#. Open \https://`<ERA-VM-IP>`:8443/ in a new browser tab.

#. Login using the following credentials:

   - **Username** - admin
   - **Password** - `<CLUSTER-PASSWORD>`

#. From the **Dashboard** drop-down, select **Administration**.

#. Under **Cluster Details**, note that Era has already been configured for your assigned cluster.

   .. figure:: images/6.png

#. From the drop-down menu, select **SLAs**.

   Era has five built-in SLAs: Gold, Silver, Bronze, Brass, and Zero. SLAs control how the database server is backed up, or in the case of the *Zero* SLA, excluded from being backed up entirely. Backups can be configured with a combination of Continuous Protection, Daily, Weekly, Monthly, and Quarterly protection intervals.

#. From the drop-down menu, select **Profiles**.

   Profiles pre-define resources and configurations, making it simple to consistently provision environments and reduce configuration sprawl. For example, a *Compute* Profile specifies the size of the database server, including details such as vCPUs, cores per vCPU, and memory.

Register Oracle Server with Era
+++++++++++++++++++++++++++++++

In this exercise, you will register your Oracle VM as version 1.0 of your Oracle 19c Software Profile. The Software Profile is a template containing both the operating system and database software, and can be used to deploy additional database servers.

#. Within **Era**, select **Database Server VMs** from the drop-down menu, and then **List** from the left-hand menu.

#. Click **+ Register > Oracle**, fill out the following fields, and click **Register**.

   - **IP Address or Name of VM** - *USERXX*\ **-Oracle19cSource**
   - **Database Version** - 19.0.0.0
   - **Era Drive User** - oracle
   - **Oracle Database Home** - /u02/app/oracle/product/19.0.0/dbhome_1
   - **Grid Infrastructure Home** - /u01/app/19.0.0/grid
   - **Provide Credentials Through** - Password
   - **Password** - Nutanix/4u

   .. figure:: images/2.png

   .. note::

      The Era Drive User can be any user on the VM that has sudo access with NOPASSWD setting. Era will use this user's credentials to perform various operations, such as taking snapshots.

      Oracle Database Home is the directory where the Oracle database software is installed, and is a mandatory parameter for registering a database server.

      Grid Infrastructure Home is the directory where the Oracle Grid Infrastructure software is installed. This is only applicable for Oracle RAC or SIHA databases.

#. If you see a similar warning as below, you can safely ignore it.

   .. figure:: images/2a.png

#. Select **Operations** from the drop-down menu to monitor the registration. This process should take approximately 5 minutes. Wait for the registration operation to successfully complete before proceeding.

   Once the *Initials*\ **_oracle_base** database has been registered within Era from your *USERXX*\ **-Oracle19cSource** VM, we need to create a software profile in order to deploy additional copies of this Oracle database.

#. Select **Profiles** from the drop-down menu, and then **Software** from the left-hand menu.

#. Click **+ Create > Oracle > Single Instance Database** and fill out the following fields:

   - **Name** - *Initials*\ _ORACLE_19C
   - **Description** - (Optional)
   - **Database Server** - Select your registered *USERXX*\ **-Oracle19cSource**

#. Click **Next > Create**.

#. Select **Operations** from the drop-down menu to monitor the registration. This process should take approximately 5 minutes.

Register Your Database
++++++++++++++++++++++

#. In **Era**, select **Databases** from the drop-down menu, and then **Sources** from the left-hand menu.

   .. figure:: images/11.png

#. Click **+ Register > Oracle > Single Instance Database** and fill out the following fields:

   - **Database is on a Server that is:** - Registered
   - **Registered Database Servers** - Select your registered *USERXX*\ **-Oracle19cSource**

#. Click **Next**.

   - **Database Name in Era** - *Initials*\ -orcl
   - **SID** - orcl19c

   .. note::

     The Oracle System ID (SID) is used to uniquely identify a particular database on a system. For this reason, one cannot have more than one database with the same SID on a computer system. When using RAC, all instances belonging to the same database must have unique SIDs.

   .. figure:: images/13.png

#. Click **Next**

   - **Name** - *Initials*\ -orcl_TM
   - **SLA** - DEFAULT_OOB_BRASS_SLA (no continuous replay)

   .. figure:: images/14.png

#. Click **Register**

#. Select **Operations** from the drop-down menu to monitor the registration. This process should take approximately 5 minutes.
