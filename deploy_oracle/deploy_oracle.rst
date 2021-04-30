.. _deploy_oracle:

-----------------
Deploying Oracle
-----------------

Traditional database VM deployment resembles the diagram below. The process generally starts with an IT ticket for a database (from Dev, Test, QA, Analytics, etc.). Next, one or more teams will need to deploy the storage resources, and VM(s) required. Once the infrastructure portion is ready, a database administrator (DBA) needs to provision, and configure database software. Once provisioned, any best practices and data protection/backup policies need to be applied. Finally, the database can be handed over to the end user. That's a lot of handoffs, potential for a lot of friction, including human error or even inconsistency.

.. figure:: images/0.png

Whereas with a Nutanix cluster and Era, provisioning and protecting a database should take you no longer than it took to read this intro.

Source Oracle VM
++++++++++++++++

In this lab you will deploy an Oracle VM, by cloning a source Oracle 19c source VM. This VM will act as a gold image to create a profile for deploying additional Oracle VMs using Era.

This VM is running Oracle 19c with April PSU patches applied.

#. In **Prism Central**, select :fa:`bars` **> Virtual Infrastructure > VMs**.

   .. figure:: images/1.png

#. Select the checkbox for *USERXX*\ **-Oracle19cSource**, and click **Actions > Clone**.

   .. figure:: images/1b.png

#. Fill out the following fields:

   - **Number Of Clones** - 1 (default)
   - **Name** - *USERXX-Oracle19cSource_*\ **Patched**
   - **vCPU(s)** - 2 (default)
   - **Number of Cores per vCPU** - 1 (default)
   - **Memory** - 8 GiB (default)

#. Click **Save** to create the VM.

#. Select your *USERXX-Oracle19cSource_*\ **Patched** VM, and click **Actions > Power On**.

Exploring Era Resources
+++++++++++++++++++++++

Era is distributed as a virtual appliance that can be installed on either AHV or ESXi. A shared Era server has already been deployed on your cluster.

.. note::

   If you're interested, instructions for the brief installation of the Era appliance can be found `here <https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Era-User-Guide-v2_1:era-era-installing-on-ahv-t.html>`_.

#. Within **Prism Central > Virtual Infrastructure > VMs > List**, identify the IP address assigned to the *Era* VM using the *IP Addresses* column.

#. Open \http://*ERA-VM-IP*/ in a new browser tab.

#. Login using the following credentials:

   - **Username** - admin
   - **Password** - <CLUSTER-PASSWORD>

#. From the **Dashboard** drop-down, select **Administration**.

#. Within the *Era Service VMs* section, note that Era has already been configured for your assigned cluster.

   .. figure:: images/6.png

.. #. Select **Networks** from the left-hand menu.
..
.. #. Select **Secondary** VLAN, and then click **Add**.
..
..    .. note::
..
..       Leave **Manage IP Address Pool** unchecked, as we will be leveraging the cluster's IPAM to manage addresses.
..
..    .. figure:: images/era_networks_001.png

#. From the drop-down menu, select **SLAs**.

   .. figure:: images/7a.png

   Era has five built-in Service Level Agreements - SLAs (Gold, Silver, Bronze, Brass, and None). SLAs control how the database is backed up. This can be with a combination of Continuous, Daily, Weekly, Monthly, and Quarterly protection intervals.

#. From the drop-down menu, select **Profiles**.

   Profiles pre-define resources and configurations, making it simple to consistently provision environments and reduce configuration sprawl. For example, *Compute* profiles specify the size of the database server, including details such as vCPUs, cores per vCPU, and memory.

.. #. If you do not see any networks defined under **Network**, click **+ Create > Oracle > Database Server VMs**.
..
..    .. figure:: images/8.png
..
.. #. Fill out the following fields and click **Create**:
..
..    - **Name** - Primary_ORACLE_NETWORK
..    - **Nutanix Cluster** - EraCluster
..    - **Public Service vLAN** - Secondary
..
..    .. figure:: images/9.png

Register Oracle Server with Era
+++++++++++++++++++++++++++++++

In this exercise, you will register version 1.0 of your Oracle 19c Software Profile. The Software Profile is a template containing both the operating system and database software, and can be used to deploy additional database servers.

#. In **Era**, select **Database Servers VMs** from the drop-down menu, and then **List** from the left-hand menu.

#. Click **+ Register > Oracle**, then fill out the following *Register Database Server VMs* fields:

   - **Nutanix Cluster** - EraCluster
   - **IP Address or Name of VM** - *USERXX*\ **-Oracle19cSource**
   - **Listener Port** - 1521 (default)
   - **Era Drive User** - `oracle`
   - **Oracle Database Home** - `/u02/app/oracle/product/19.0.0/dbhome_1`
   - **Grid Infrastructure Home** - `/u01/app/19.0.0/grid`
   - **Provide Credentials Through** - Password
   - **Password** - `Nutanix/4u`

   .. note::

      The *Era Drive User* can be any user on the VM that has sudo access with NOPASSWD setting. Era will use this user's credentials to perform various operations, such as taking snapshots.

      *Oracle Database Home* is the directory where the Oracle database software is installed, and is a mandatory parameter for registering a database server.

      *Grid Infrastructure Home* is the directory where the Oracle Grid Infrastructure software is installed. This is only applicable for Oracle RAC or SIHA databases.

   .. figure:: images/2.png

#. Click **Register**.

#. Take note of *Missing Dependencies/Configurations* dialogue box. In this instance, *sshpass* is only required for Oracle RAC Patching, and can safely be ignored (i.e. warning for soft dependencies). Era provides these built-in checks to ensure a clean experience moving forward.

   .. figure:: images/2a.png

#. Click **Close**.

#. Select **Operations** from the drop-down menu to monitor the progress. This process should take <5 minutes. Please wait for the *Register Database Server VM* operation to successfully complete before moving on to the next step.

   Once the *USERXX*\ **-Oracle19cSource** server has been registered with Era, we need to create a software profile in order to deploy additional Oracle VMs.

#. Select **Profiles** from the drop-down menu, and then **Software** from the left-hand menu.

#. Click **+ Create > Oracle > Single Instance Database**, and then fill out the following fields:

   - **Profile Name** - *USERXX*\ _ORACLE_19C
   - **Profile Description** - (Optional)
   - **Software Profile Version Name** - USERXX_ORACLE_19C (1.0) (default)
   - **Software Profile Version Description** - (Optional)
   - **Nutanix Cluster** - EraCluster
   - Select your registered *USERXX*\ **-Oracle19cSource** VM

   .. figure:: images/3.png

#. Click **Next**.

#. Add Software Profile Notes – (Optional).

#. Click **Create**.

#. Select **Operations** from the drop-down menu to monitor the registration. This process should take <5 minutes. Please wait for the *Create Software Profile* operation to successfully complete before moving on to the next step.

Register Your Database
++++++++++++++++++++++

#. Within **Era**, select **Databases** from the drop-down menu, and then **Sources** from the left-hand menu.

   .. figure:: images/11.png

#. Click **+ Register > Oracle > Single Instance Database**, and then fill out the following fields:

   - **Database is on a Server VM that is:** - Registered
   - **Registered Database Server VMs** - Select your registered *USERXX*\ **-Oracle19cSource** VM

   .. figure:: images/12.png

#. Click **Next**, and then fill out the following fields:

   - **Database Name in Era** - *USERXX*\ _orcl
   - **SID** - orcl19c

   .. note::

     The Oracle System ID (SID) is used to uniquely identify a particular database on a system. For this reason, one cannot have more than one database with the same SID. When using Oracle RAC, all instances belonging to the same database must have unique SIDs.

   .. figure:: images/13.png

#. Click **Next**, and then fill out the following fields:

   - **Name** - *USERXX*\ _orcl_TM (default)
   - **SLA** - DEFAULT_OOB_GOLD_SLA

   .. figure:: images/14.png

#. Click **Register**.

#. Select **Operations** from the drop-down menu to monitor the progress. This process should take <5 minutes. Please wait for the *Register Database* operation to successfully complete before moving on to the next section.
