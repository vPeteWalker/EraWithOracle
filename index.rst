.. title:: Databases: Era with Oracle Bootcamp

.. toctree::
   :maxdepth: 2
   :caption: Era with Oracle
   :name: _dbs
   :hidden:

   deploy_oracle/deploy_oracle
   deploy_oracle_era/deploy_oracle_era
   admin_oracle/admin_oracle
   patching_oracle/patching_oracle

.. toctree::
  :maxdepth: 2
  :caption: Optional Labs
  :name: _optional_labs
  :hidden:

  era_rest_api/era_rest_api

.. toctree::
  :maxdepth: 2
  :caption: Appendix
  :name: _appendix
  :hidden:

  appendix/glossary
..  tools_vms/windows_tools_vm
  tools_vms/linux_tools_vm
  labsetup/labsetup


.. _getting_started:

---------------
Getting Started
---------------

Welcome to the *Databases: Era with Oracle* bootcamp. This bootcamp is meant to provide you with first-hand experience in why Nutanix is an ideal platform for database workloads.

Historically, it has been a challenge to virtualize Oracle because of the high cost of traditional virtualization stacks, and the impact that a SAN-based architecture can have on performance. Businesses and their IT departments have constantly fought to balance cost, operational simplicity, consistent, and predictable performance.

The Nutanix Enterprise Cloud removes many of these challenges, and makes virtualizing a business-critical application such as Oracle much easier. The Acropolis Distributed Storage Fabric (ADSF) is a software-defined solution that provides all the features one typically expects in an enterprise SAN, without a SAN’s physical limitations and bottlenecks. Oracle particularly benefits from the following DSF features:

- Localized I/O, and the use of flash for index and key database files = lower operation latency.
- A highly distributed approach that can handle both random and sequential workloads.
- The ability to add new nodes, and scale the infrastructure without either system downtime or performance impact.
- Nutanix data protection and disaster recovery workflows that simplify backup operations and business continuity processes.

In addition to solving common infrastructure problems for hosting business critical applications, Nutanix also seeks to address many of the key pain points associated with managing databases.

.. figure:: images/4.png

Based on a 2018 IDC study of 500 North American companies with more than 1,000 employees, they estimate:

- 77% of the organizations have more than 200 database instances in their production.
- 82% have more than 10 copies of each DB.
- 45%-60% the total storage capacity is dedicated to accommodating copy data.
- 32% of database clones require daily refreshes for analytics of dev/test.
- Copy data will cost IT organizations $55.63 billion in 2020.

Maintaining the status quo leads to inefficient usage of both storage, and an administrator's time.

   Meet Nutanix Era.

.. figure:: images/5.png

Nutanix Era provides DBaaS for your Enterprise Cloud. Leveraging the Nutanix Enterprise Cloud OS, we are able to take advantage of the power of full stack - data, compute, and software. Through common APIs, CLI, and a consumer-grade GUI experience for multiple database engines, Nutanix Era eliminates the complexity of database operations. Database operations (ex. cloning) are more efficient, thereby driving down the TCO of database management for our customers.

What's New
++++++++++

- Workshop updated using the following software versions:
    - AOS 5.19.1.6
    - Prism 2021.3.0.1
    - Era 2.1.1.2

.. - Optional Lab Updates:

Agenda
++++++

- Introductions
- Lab Setup
- Deploy Oracle
- Deploy Oracle with Era
- DB Administration with Era
- Patching Oracle with Era

Optional labs:

- Era REST API Explorer

Introductions
+++++++++++++

- Name
- Familiarity with Nutanix

Initial Setup
+++++++++++++

- Take note of the *Passwords* being used.
- Log into your virtual desktops (connection info below).

Cluster assignment
++++++++++++++++++

The instructor will advise the attendees of their assigned clusters.

Environment Details
+++++++++++++++++++

Nutanix workshops are intended to be run in the Nutanix Hosted POC (HPOC) environment. Your cluster will be provisioned with all necessary images, networks, and VMs required to complete the exercises.

Credentials
...........

.. note::

  The *<Cluster Password>* is unique to each cluster, and will be provided by the leader of the workshop.

Each cluster has a dedicated domain controller VM, responsible for providing Active Directory services for the *NTNXLAB.local* domain. The domain is populated with the following users and groups:

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Group
     - Username(s)
     - Password
   * - Administrators
     - Administrator
     - nutanix/4u
   * - SSP Admins
     - adminuser01-adminuser25
     - nutanix/4u
   * - SSP Developers
     - devuser01-devuser25
     - nutanix/4u
   * - SSP Consumers
     - consumer01-consumer25
     - nutanix/4u
   * - SSP Operators
     - operator01-operator25
     - nutanix/4u
   * - SSP Custom
     - custom01-custom25
     - nutanix/4u
   * - Bootcamp Users
     - user01-user25
     - nutanix/4u

Access Instructions
+++++++++++++++++++

The Nutanix Hosted POC environment can be accessed a number of different ways:

Lab Access User Credentials
...........................

PHX Based Clusters:
**Username:** PHX-POCxxx-User01 (up to PHX-POCxxx-User20), **Password:** *<Provided by Instructor>*

RTP Based Clusters:
**Username:** RTP-POCxxx-User01 (up to RTP-POCxxx-User20), **Password:** *<Provided by Instructor>*

Frame VDI
.........

Login to: https://console.nutanix.com/x/labs

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Parallels VDI
.................

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Employee Pulse Secure VPN
..........................

Download the client:

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Install the client.

In Pulse Secure Client, **Add** a connection:

For PHX:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - PHX
- **Server URL** - xlv-uswest1.nutanix.com

For RTP:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - RTP
- **Server URL** - xlv-useast1.nutanix.com
