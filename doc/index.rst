====================
Azure SDK for Python
====================

The Azure SDK for Python is a set of libraries which allow you to work on Azure for your management, runtime or data needs.

For a more general view of Azure and Python, you can go on the `Python Developer Center for Azure <https://azure.microsoft.com/en-us/develop/python/>`_

Example Usage
-------------

This example shows:

* Authentication on Azure using an AD in your subscription,
* Creation of a Resource Group and a Storage account,
* Upload a simple "Hello world" HTML page and gives you the URL to get it.

.. code:: python

    from azure.common.credentials import UserPassCredentials
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.storage import StorageManagementClient
    from azure.storage import CloudStorageAccount
    from azure.storage.blob.models import ContentSettings

    credentials = UserPassCredentials('user@domain.com', 'my_smart_password')
    subscription_id = '33333333-3333-3333-3333-333333333333'

    resource_client = ResourceManagementClient(credentials, subscription_id)
    storage_client = StorageManagementClient(credentials, subscription_id)

    resource_client.resource_groups.create_or_update(
        'my_resource_group',
        {
            'location':'westus'
        }
    )

    async_create = storage_client.storage_accounts.create(
        'my_resource_group',
        'my_storage_account',
        {
            'location':'westus',
            'account_type':'Standard_LRS'
        }
    )
    async_create.wait()
    
    storage_keys = storage_client.storage_accounts.list_keys('my_resource_group', 'my_storage_account')
    storage_keys = {v.key_name: v.value for v in storage_keys.keys}

    storage_client = CloudStorageAccount('my_storage_account', storage_keys['key1'])
    blob_service = storage_client.create_block_blob_service()

    blob_service.create_container('my_container_name')

    blob_service.create_blob_from_bytes(
        'my_container_name',
        'my_blob_name',
        b'<center><h1>Hello World!</h1></center>',
        content_settings=ContentSettings('text/html')
    )

    print(blob_service.make_blob_url('my_container_name', 'my_blob_name'))



Installation
------------

You can install the whole set of stable Azure libraries in a single line using the ``azure`` meta-package:

.. code-block:: console

   $ pip install azure

**Important Note: If you want to use Azure Resource Management (ARM), there is no stable release of the azure meta-package yet.**

However, the core packages, from code quality/completeness perspectives can at this time be considered "stable" 
- it will be officially labeled as such in sync with other languages as soon as possible. 
We are not planning on any further major changes until then.**

We then strongly recommend to install ``azure`` 2.0.0rc6 to use ARM. Since it's a preview release, you need to use the ``--pre`` flag:

.. code-block:: console

   $ pip install --pre azure
   
or directly

.. code-block:: console

   $ pip install azure==2.0.0rc6
   
You can also install individually each library if you don't need everything
and want to save installation space/time.

.. code-block:: console

   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library

Preview packages are not included in the ``azure`` meta-package and can be installed using the ``--pre`` flag:

.. code-block:: console

   $ pip install --pre azure-mgmt-web # will install only the latest Web App Management library

More details and information about the available libraries and their status can be found 
in the :doc:`Installation Page<installation>`


Features
--------

Azure Resource Management
^^^^^^^^^^^^^^^^^^^^^^^^^

All documentation of management libraries for Azure are on this website. This includes:

* :doc:`Authorization <resourcemanagementauthorization>` : Permissions, roles and more
* :doc:`Batch<resourcemanagementbatch>` : Manage Batch accounts and applications
* :doc:`Content Delivery Network<resourcemanagementcdn>` : Profiles, endpoints creation and more
* :doc:`Commerce - Billing API<resourcemanagementcommerce>` : RateCard and Usage Billing API
* :doc:`Compute<resourcemanagementcomputenetwork>` : Create virtual machines and more
* :doc:`App Service<resourcemanagementapps>` : Manage App plan, Web Apps, Logic Apps and more
* :doc:`Network<resourcemanagementcomputenetwork>` : Create virtual networks, network interfaces, public IPs and more
* :doc:`Notification Hubs<resourcemanagementnotificationhubs>` : Namespaces, hub creation/deletion and more
* :doc:`Redis Cache<resourcemanagementredis>` : Create cache and more
* :doc:`Resource Management<resourcemanagement>`:

  * resources :  create resource groups, register providers and more
  * features : manage features of provider and more
  * locks : manage resource group lock and more
  * subscriptions : manage subscriptions and more
  
* :doc:`Scheduler<resourcemanagementscheduler>` : Create job collections, create job and more
* :doc:`Storage<resourcemanagementstorage>` : Create storage accounts, list keys, and more

Azure Runtime
^^^^^^^^^^^^^

Some documentation of data libraries are on this website. This includes:

* :doc:`Batch<batch>`
* :doc:`Azure Active Directory Graph RBAC<graphrbac>`
* :doc:`Service Bus<servicebus>` using HTTP.

  Note that for critical performance issue, the Service Bus team is currently recommended `AMQP <https://azure.microsoft.com/en-us/documentation/articles/service-bus-amqp-python/>`_.

These Azure services have Python data libraries which are directly hosted by the service team or are extensively documented on the Azure documentation website:

* `Storage <http://azure-storage.readthedocs.org>`_
* `Azure IoT device SDK for Python <https://github.com/Azure/azure-iot-sdks/tree/master/python/device>`_
* `SQL Azure <https://azure.microsoft.com/en-us/documentation/articles/sql-database-develop-python-simple/>`_
* `DocumentDB <https://azure.microsoft.com/en-us/documentation/articles/documentdb-sdk-python/>`_
* `Application Insight <https://github.com/Microsoft/ApplicationInsights-Python>`_
* `Redis Cache <https://azure.microsoft.com/en-us/documentation/articles/cache-python-get-started/>`_
* `Write an Azure WebApp in Python <https://azure.microsoft.com/en-us/documentation/articles/web-sites-python-create-deploy-django-app/>`_


Azure Service Management
^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: The Service Management SDK is deprecated and no more features will be added.

This page describes the :doc:`usage and detailed features of Azure Service Management SDK<servicemanagement>`. At a glance:

   -  storage accounts: create, update, delete, list, regenerate keys
   -  affinity groups: create, update, delete, list, get properties
   -  locations: list
   -  hosted services: create, update, delete, list, get properties
   -  deployment: create, get, delete, swap, change configuration,
      update status, upgrade, rollback
   -  role instance: reboot, reimage
   -  discover addresses and ports for the endpoints of other role
      instances in your service
   -  get configuration settings and access local resources
   -  get role instance information for current role and other role
      instances
   -  query and set the status of the current role

System Requirements:
--------------------

The supported Python versions are 2.7.x, 3.3.x, 3.4.x, and 3.5.x
To download Python, please visit
https://www.python.org/download/


We recommend Python Tools for Visual Studio as a development environment for developing your applications.  Please visit http://aka.ms/python for more information.


Need Help?:
-----------

Be sure to check out the Microsoft Azure `Developer Forums on Stack
Overflow <http://go.microsoft.com/fwlink/?LinkId=234489>`__ if you have
trouble with the provided code.

Contribute Code or Provide Feedback:
------------------------------------

If you would like to become an active contributor to this project please
follow the instructions provided in `Microsoft Azure Projects
Contribution
Guidelines <http://windowsazure.github.com/guidelines.html>`__.

If you encounter any bugs with the library please file an issue in the
`Issues <https://github.com/Azure/azure-sdk-for-python/issues>`__
section of the project.


Indices and tables
------------------

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

.. toctree::
  :glob:
  :caption: User Documentation

  installation
  quickstart_authentication
  
.. toctree::
  :glob:
  :caption: Management Documentation

  resourcemanagement*
  Service Management (Legacy) <servicemanagement>
  
.. toctree::
  :glob:
  :caption: Runtime Documentation

  batch
  graphrbac
  servicebus
  
.. toctree::
  :maxdepth: 5
  :glob:
  :caption: Developer Documentation

  ref/azure.common
  ref/azure.batch
  ref/azure.graphrbac
  ref/azure.mgmt.authorization
  ref/azure.mgmt.batch
  ref/azure.mgmt.cdn
  ref/azure.mgmt.cognitiveservices
  ref/azure.mgmt.commerce
  ref/azure.mgmt.compute
  ref/azure.mgmt.dns
  ref/azure.mgmt.iothub
  ref/azure.mgmt.keyvault
  ref/azure.mgmt.logic
  ref/azure.mgmt.network
  ref/azure.mgmt.notificationhubs
  ref/azure.mgmt.powerbiembedded
  ref/azure.mgmt.redis
  ref/azure.mgmt.resource
  ref/azure.mgmt.scheduler
  ref/azure.mgmt.storage
  ref/azure.mgmt.trafficmanager
  ref/azure.mgmt.web
  ref/azure.servicebus
  ref/azure.servicemanagement
