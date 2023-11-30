===========================================
Welcome to the documentation of Pricing-API
===========================================


1. Stats
========
Document status: 2023-01-24

Version: 0.5

End points:

https://calculator.otc-services.com/en/open-telekom-price-api/  

https://calculator.otc-services.com/en/open-telekom-price-api-ui/ 

https://calculator.otc-services.com/de/open-telekom-price-api/    

https://calculator.otc-services.com/de/open-telekom-price-api-ui/ 



2. Parameters
=============

The values can be sent via POST or GET.

At first a checksum is formed from the parameters, which is also used in parts as a key for the cache
entry. So the cached data is returned, if there is a valid cache entry. Extensive validation of the
entries can then be omitted.

All entries are validated, filtered and grouped by service, if no cache entry for the respective request
was found. A pagination structure is also supplied for very large amounts of data.

2.1.Validation
--------------
• Numerical values are always cast to an <Int>
• The values of responseFormat, serviceName, groupBy, region and columns as well as the keys of filterBy are compared with the stored identifiers, settings or with the field names in the database schema. This eliminates any values that are beyond the technical realities.
• Only the value of filterBy must be filtered and escaped



2.2.Overview
------------


+-----------------------------------+------------------------+-------------------------+----------------------+----------------------------------+--------------------------------------------+
| Name                              | Alias                  |  Required               |  Type                |  Description                     |   Example                                  |                 
+===================================+========================+=========================+======================+==================================+============================================+
|      region                       |   rn                   |      no                 |    String            | Determines which products        |   region=eu-de                             |                    
|                                   |                        |                         |    Array             | are loaded in which region.      |                                            |
|                                   |                        |                         |                      |                                  |                                            |                    
|                                   |                        |                         |                      | Possible regions – see chapter   |   region[]=eu-de&                          |                    
|                                   |                        |                         |                      | 3. Permitted values. All         |   region[]=eu-nl                           |
|                                   |                        |                         |                      | regions of the respective        |                                            |
|                                   |                        |                         |                      | calculator are loaded, if the    |                                            |                    
|                                   |                        |                         |                      | value is empty.                  |                                            | 
+-----------------------------------+------------------------+-------------------------+----------------------+----------------------------------+--------------------------------------------+
|      serviceName                  |   sn                   |      no                 |    String            | String or array with the         |   serviceName=ecs                          |   
|                                   |                        |                         |    Array             | service name (s) that are used   |                                            |                
|                                   |                        |                         |                      | for a meaningful table display   |   serviceName []=ecs&                      |                    
|                                   |                        |                         |                      |                                  |   serviceName []=bms                       | 
+-----------------------------------+------------------------+-------------------------+----------------------+----------------------------------+--------------------------------------------+
|                                   |                        |                         |                      | The order can be important for   |                                            |                   
|                                   |                        |                         |                      | the presentation.                |                                            |
|                                   |                        |                         |                      |                                  |                                            |                    
|                                   |                        |                         |                      | Possible service names - see     |                                            |                    
|                                   |                        |                         |                      | chapter 34. Permitted values.    |                                            |
|                                   |                        |                         |                      | All services of the respective   |                                            |
|                                   |                        |                         |                      | calculator are loaded, if the    |                                            |                    
|                                   |                        |                         |                      | value is empty.                  |                                            | 
+-----------------------------------+------------------------+-------------------------+----------------------+----------------------------------+--------------------------------------------+
|     filterBy[column][]            |     fb                 |      no                 |       Array          | Array with values by which the   | filterBy[region][1]=eude&                  |
|                                   |                        |                         |                      | queried data records have to be  | filterBy[region][2]=eunl&                  |
|                                   |                        |                         |                      | filtered. The key of the array   | filterBy[osUnit]=Windows                   |            
|                                   |                        |                         |                      | item corresponds to the column   |                                            |                
|                                   |                        |                         |                      | name in the product table or the |                                            |                    
|                                   |                        |                         |                      | header in the import Excel file. |                                            |
|                                   |                        |                         |                      |                                  |                                            |                    
|                                   |                        |                         |                      | Possible column values - see: 3. |                                            |            
|                                   |                        |                         |                      | Permitted values.                |                                            |
+-----------------------------------+------------------------+-------------------------+----------------------+----------------------------------+--------------------------------------------+
|     groupBy                       |     gb                 |      no                 |       String         | A column name by which all       | groupBy=unit                               |
|                                   |                        |                         |                      | entries can be grouped again.    |                                            | 
|                                   |                        |                         |                      | This grouping is not identical   |                                            |            
|                                   |                        |                         |                      | to the SQL grouping              |                                            |                
|                                   |                        |                         |                      |                                  |                                            |                    
|                                   |                        |                         |                      | Possible column values - see: 3. |                                            |            
|                                   |                        |                         |                      | Permitted values.                |                                            |
+-----------------------------------+------------------------+-------------------------+----------------------+----------------------------------+--------------------------------------------+
|     columns                       |     cs                 |      no                 |       Array          | Array with the column names      | columns[]=osUnit&                          |
|                                   |                        |                         |                      | that are used for a meaningful   | columns[]=id&                              |
|                                   |                        |                         |                      | table display. The order can be  | columns[]=priceAmount                      |            
|                                   |                        |                         |                      | important for the presentation   |                                            |                
|                                   |                        |                         |                      |                                  |                                            |                    
|                                   |                        |                         |                      | Possible column values - see:    |                                            |            
|                                   |                        |                         |                      | chapter 3. Permitted values. All |                                            |
|                                   |                        |                         |                      | columns of a data record defined |                                            |
|                                   |                        |                         |                      | by default are returned, if the  |                                            |
|                                   |                        |                         |                      | value is empty.                  |                                            |
+-----------------------------------+------------------------+-------------------------+----------------------+----------------------------------+--------------------------------------------+
|     filters                       |     fi                 |      no                 |       Array          | Array with the column names      | filters[]=osUnit&                          |
|                                   |                        |                         |                      | that are used for building       | filters[]=id                               | 
|                                   |                        |                         |                      | excel like filters               |                                            |            
+-----------------------------------+------------------------+-------------------------+----------------------+----------------------------------+--------------------------------------------+
|     limitFrom                     |     lf                 |      no                 |       Int            | Numeric value to indicate the    | limitFrom=100                              |
|                                   |                        |                         |                      | position from which the record   |                                            |
|                                   |                        |                         |                      | are loaded. This is helpful for  |                                            |   
|                                   |                        |                         |                      | realizing **pagination**         |                                            |                
|                                   |                        |                         |                      |                                  |                                            |                    
|                                   |                        |                         |                      | It always starts with the first  |                                            |            
|                                   |                        |                         |                      | search hit, if the value is      |                                            |
|                                   |                        |                         |                      | empty.                           |                                            |
+-----------------------------------+------------------------+-------------------------+----------------------+----------------------------------+--------------------------------------------+
|     limitMax                      |     lm                 |      no                 |       Int            | Numeric value to determine how   | limitMax=250                               |
|                                   |                        |                         |                      | many data records should be      |                                            |
|                                   |                        |                         |                      | loaded. This is helpful for      |                                            |   
|                                   |                        |                         |                      | realizing a **pagination**, but  |                                            |             
|                                   |                        |                         |                      | can also be used to limit the    |                                            |   
|                                   |                        |                         |                      | amount of data records.          |                                            |            
|                                   |                        |                         |                      |                                  |                                            |
|                                   |                        |                         |                      | The technically resonable        |                                            |
|                                   |                        |                         |                      | maximum of data is loaded, if the|                                            |
|                                   |                        |                         |                      | value is empty. A maximum of 500 |                                            | 
|                                   |                        |                         |                      | are currently employed at the    |                                            |  
|                                   |                        |                         |                      | same time.                       |                                            | 
+-----------------------------------+------------------------+-------------------------+----------------------+----------------------------------+--------------------------------------------+
|     responseFormat                |     rf                 |      no                 |       String         | The output format can be         | responseFormat=                            |
|                                   |                        |                         |                      | determined with this value.      |                                            | 
|                                   |                        |                         |                      |                                  | responseFormat=xml                         |             
|                                   |                        |                         |                      | The data is returned in a JSON   |                                            |            
|                                   |                        |                         |                      | structure, if the value is empty.| responseFormat=html(debug)                 |
+-----------------------------------+------------------------+-------------------------+----------------------+----------------------------------+--------------------------------------------+
|     nocache                       |     nc                 |      no                 |       String         | The parameter deactivates        | nocache=1                                  |
|                                   |                        |                         |                      | caching for this request so that |                                            | 
|                                   |                        |                         |                      | current data is always loaded.   |                                            |   
+-----------------------------------+------------------------+-------------------------+----------------------+----------------------------------+--------------------------------------------+


3. Permitted values
===================

3.1 Overview of regions
---------------------------
+-----------------------------------+------------------------+-------------------------+
| Calculator                        | Type                   |  Example                |                 
+===================================+========================+=========================+
|      OTC                          |  string                | eu-de                   |    
+-----------------------------------+------------------------+-------------------------+
|                                   |  string                | eu-nl                   |  
+-----------------------------------+------------------------+-------------------------+



3.2.4.3. Overview of column names
---------------------------------


In the case of column names (**columns**, **filters** and keys for **filterBy** or as values for **groupBy**), only
values that are available in the table schema are allowed. All column names that are suitable for
filtering are listed below.



+----------------+----------+--------+-----------------------------+
| Column name    | Type     | Length | Example                     |
+================+==========+========+=============================+
| id             | varchar  | 255    | OTC_OBSWM_SP_1              |
+----------------+----------+--------+-----------------------------+
| osUnit         | varchar  | 255    | OpenLinux                   |
+----------------+----------+--------+-----------------------------+
| productid      | varchar  | 255    | WARM OBJECT STORAGE SERVICE |
+----------------+----------+--------+-----------------------------+
| productName    | varchar  | 255    | Warm Object Storage - Space |
+----------------+----------+--------+-----------------------------+
| currency       | varchar  | 255    | EUR                         |
+----------------+----------+--------+-----------------------------+
| priceAmount    | double   | 20     | 0.046                       |
+----------------+----------+--------+-----------------------------+
| unit           | varchar  | 255    | GB                          |
+----------------+----------+--------+-----------------------------+
| vCpu           | varchar  | 255    | 32                          |
+----------------+----------+--------+-----------------------------+
| ram            | varchar  | 255    | 16                          |
+----------------+----------+--------+-----------------------------+
| additionalText | longtext |        |                             |
+----------------+----------+--------+-----------------------------+
| storageType    | varchar  | 255    |                             |
+----------------+----------+--------+-----------------------------+
| storageVolume  | varchar  | 255    |                             |
+----------------+----------+--------+-----------------------------+
| serviceType    | varchar  | 255    | General Purpose v1          |
+----------------+----------+--------+-----------------------------+
| description    | varchar  | 255    |                             |
+----------------+----------+--------+-----------------------------+
| opiFlavour     | varchar  | 255    | s2.medium.4                 |
+----------------+----------+--------+-----------------------------+
| region         | varchar  | 255    | eu-de                       |
+----------------+----------+--------+-----------------------------+






3.3.Overview of services
------------------------



In order to load only one or a selection of services, appropriate identifiers can be transferred to the
services using **serviceName**. Their names are analogous to those from the console. All service names
that are suitable for filtering are listed below.




+------------------------------------------+--------+------------+
| Servicename                              | Typ    | Wert       |
+==========================================+========+============+
| Application Operations Management        | String | aom        |
+------------------------------------------+--------+------------+
| Bare Metal Service                       | String | bms        |
+------------------------------------------+--------+------------+
| Cloud Backup and Recovery                | String | cbr        |
+------------------------------------------+--------+------------+
| Cloud Container Engine                   | String | cce        |
+------------------------------------------+--------+------------+
| Cloud Search Service                     | String | css        |
+------------------------------------------+--------+------------+
| Cloud Search Client Node                 | String | csscln     |
+------------------------------------------+--------+------------+
| Cloud Search Cold Node                   | String | csscon     |
+------------------------------------------+--------+------------+
| Cloud Search Master Node                 | String | cssman     |
+------------------------------------------+--------+------------+
| Cloud Server Backup Service              | String | csbs       |
+------------------------------------------+--------+------------+
| Data Admin Service                       | String | das        |
+------------------------------------------+--------+------------+
| Data ingestion Service                   | String | dis        |
+------------------------------------------+--------+------------+
| Data Replication Service                 | String | drs        |
+------------------------------------------+--------+------------+
| Data Warehouse Service                   | String | dws        |
+------------------------------------------+--------+------------+
| Dedicated Host                           | String | deh        |
+------------------------------------------+--------+------------+
| Dedicated Host License                   | String | dehl       |
+------------------------------------------+--------+------------+
| Direct Connect                           | String | dc         |
+------------------------------------------+--------+------------+
| Direct Connect Setup                     | String | dcsetup    |
+------------------------------------------+--------+------------+
| Disk Intensive Service                   | String | dins       |
+------------------------------------------+--------+------------+
| Distributed Cache Service                | String | dcs        |
+------------------------------------------+--------+------------+
| Distributed Cache Service Backup         | String | dcsb       |
+------------------------------------------+--------+------------+
| Distributed Message Service              | String | dms        |
+------------------------------------------+--------+------------+
| Distributed Message Service Kafka        | String | dmsk       |
+------------------------------------------+--------+------------+
| Distributed Message Service Public IP    | String | dmsip      |
+------------------------------------------+--------+------------+
| Distributed Message Service Volume       | String | dmsvol     |
+------------------------------------------+--------+------------+
| DNS Private Queries                      | String | dnprq      |
+------------------------------------------+--------+------------+
| DNS Public Queries                       | String | dnq        |
+------------------------------------------+--------+------------+
| DNS Service Private                      | String | prhz       |
+------------------------------------------+--------+------------+
| DNS Service Public                       | String | phz        |
+------------------------------------------+--------+------------+
| Document Database Service                | String | dds        |
+------------------------------------------+--------+------------+
| Document Database Service RS             | String | ddsrs      |
+------------------------------------------+--------+------------+
| Document Database SN                     | String | ddssn      |
+------------------------------------------+--------+------------+
| Elastic Cloud Server                     | String | ecs        |
+------------------------------------------+--------+------------+
| Elastic Cloud Server (Compute Optimized) | String | ecsnoc     |
+------------------------------------------+--------+------------+
| Elastic Cloud Server (Memory Optimized)  | String | memo       |
+------------------------------------------+--------+------------+
| Elastic Cloud Server (Ultra High I/O)    | String | uhio       |
+------------------------------------------+--------+------------+
| Elastic IP Service                       | String | eip        |
+------------------------------------------+--------+------------+
| Elastic Load Balancer Service            | String | elb        |
+------------------------------------------+--------+------------+
| Elastic Volume Service                   | String | evs        |
+------------------------------------------+--------+------------+
| Enterprise Agreement                     | String | ea         |
+------------------------------------------+--------+------------+
| GPU Service                              | String | gpu        |
+------------------------------------------+--------+------------+
| High Performance Server                  | String | hps        |
+------------------------------------------+--------+------------+
| Internet Traffic Outbound Service        | String | ito        |
+------------------------------------------+--------+------------+
| Key Message Services                     | String | kms        |
+------------------------------------------+--------+------------+
| Large Memory Service                     | String | lms        |
+------------------------------------------+--------+------------+
| Large Memory Service Storage             | String | lmss       |
+------------------------------------------+--------+------------+
| Log Tank Service                         | String | Its        |
+------------------------------------------+--------+------------+
| Maas                                     | String | mas        |
+------------------------------------------+--------+------------+
| Mail IP Service                          | String | mip        |
+------------------------------------------+--------+------------+
| MapReduce Service                        | String | mrs        |
+------------------------------------------+--------+------------+
| Mobile Storage Solution                  | String | mss        |
+------------------------------------------+--------+------------+
| ModelArts                                | String | modelarts  |
+------------------------------------------+--------+------------+
| NAT Gateaway Service                     | String | nat        |
+------------------------------------------+--------+------------+
| Object Storage Service (Standard)        | String | obs        |
+------------------------------------------+--------+------------+
| Object Storage Service (Cold)            | String | coss       |
+------------------------------------------+--------+------------+
| Object Storage Service (Warm)            | String | woss       |
+------------------------------------------+--------+------------+
| Private Line Access Service              | String | plas       |
+------------------------------------------+--------+------------+
| Enterprise Financial Dashbord            | String | pefd       |
+------------------------------------------+--------+------------+
| Relational Database Backup Service       | String | rdbs       |
+------------------------------------------+--------+------------+
| Relational Database Service              | String | rds        |
+------------------------------------------+--------+------------+
| Relational Database Storage              | String | rdss       |
+------------------------------------------+--------+------------+
| Secure File Service                      | String | sfs        |
+------------------------------------------+--------+------------+
| Simple Message Services                  | String | smn        |
+------------------------------------------+--------+------------+
| Storage Disaster Recover yService        | String | sdrs       |
+------------------------------------------+--------+------------+
| Virtual Private Network                  | String | vpn        |
+------------------------------------------+--------+------------+
| Volume Backup Service                    | String | vbs        |
+------------------------------------------+--------+------------+
| VPC Endpoint                             | String | vpcep      |
+------------------------------------------+--------+------------+
| Web Application Firewall                 | String | waf        |
+------------------------------------------+--------+------------+



4. Returned values
==================


The JSON format is currently provided for error messages and to return results.


4.1. Successful request
-----------------------

With a successful standard request (without parameters) this could look like this:

.. code-block:: console

   {
    "response":{
    "url":"https://calculator.otc-services.com/open-telekom-price-api/",
    "parameters":{
    "productType":"OTC",
    "serviceName":["ecs"],
    "limitMax":"25",
    "nocache":"0"
    },
    "httpCode":200,
    "code":"Success",
    "message":"Product data successfully loaded!",
    "stats":{
    "count":122,
    "recordsCount":25,
    "maxPages":5,
    "recordsPerPage":25,
    "currentPage":1,
    "currentUri":"https://example.com/?productType=OTC...limitFrom=0"
    },
    "result":{
    "ecs":[
    {
    ...
    }
    ]
    },
    "columns":{
    "id":"ID",
    "productId":"Service ID",
    "opiFlavour":"Flavor",
    "productName":"Product name",
    "osUnit":"OS unit",
    ...
    },
    "services":{
    "recordscount":1,
    "records":{
    "ecs":{
    "title":"Elastic Cloud Server",
    "moduleType":"CloudServerServiceBundled",
    "description":"Choose from our numerous basic VM flavors.",
    "identifier":"ELASTIC CLOUD SERVER (BUNDLED)",
    "parameterIdentifier":"ecs"
    }
    }
    }
   "pagination":{
    "first":{
    "number":1,
    "disabled":true,
    "href":"https://calculator.otc-services.com/...&limitFrom=0",
    "current":true,
    "separator":false
    },
    "prev":{
    "number":1,
    "disabled":true,
    "href":" https://calculator.otc-services.com/...&limitFrom=0",
    "current":true,
    "separator":false
    },
    "numbers":[
    {
    "number":1,
    "disabled":true,
    "href":"https://calculator.otc-services.com/...&limitFrom=0",
    "current":true,
    "separator":false
    },
    {
    "number":2,
    "disabled":false,
    "href":"https://calculator.otc-services.com/...&limitFrom=25",
    "current":false,
    "separator":false
    },
    ...
    ],
    "next":{
    ...
    },
    "last":{
    "number":5,
    "disabled":false,
    "href":"https://calculator.otc-services.com/...&limitFrom=100",
    "current":false,
    "separator":false
    }
    }
    }
   }



4.2. Incorrect request
----------------------
An error response could look like this:

.. code-block:: console

   {
    "response": {
    "url": "https:...&responseFormat=json",
    "parameters": {},
    "httpCode": 500,
    "code": "Error",
    "message": "Validation error!",
    }
   }




5. Requests
===========


The values can be sent via POST or GET. The order or number of the parameters (see chapter 3) is not
relevant, since all parameters can be used optionally and independently of one another.


5.1 Errors 
----------

All parameters are validated and all errors will result in a failed request. Ignorable errors are
therefore not eliminated.


1. An error is always returned, if the request contains an unknown parameter.


2. An error is always returned, if one of the parameter values within the permitted parameters
is invalid.

.. code-block:: console

   {
    "response": {
    "url": "https:...&responseFormat=json",
    "parameters": {},
    "httpCode": 500,
    "code": "Error",
    "message": "Computer says: No!"
    }
   }





5.2. Standard request
---------------------


The standard request works without any parameters and therefore returns all product data of all
regions, grouped into the respective services.


Request: https://calculator.otc-services.com/open-telekom-price-api

.. code-block:: console

   {
    "response": {
    "cachedAt": "2020-01-27 11:25:37",
    "url": "https://calculator.otc-services.com/open-telekom-price-api",
    "parameters": {},
    "responseCode": 200,
    "code": "success",
    "message": "Success!",
    "stats": {
    ...
    },
    "result": {
    "services": {
    "ecs": {
    ...
    },
    "obs": {
    ...
    }
    }
    }
    }
   }


5.3. Single service request
---------------------------

To load the data of an individual service, only the **serviceName** (see chapter 3. Parameters) from one
of the possible services (see chapter 4. Permitted values) has to be specified.


Request: https://calculator.otc-services.com/open-telekom-priceapi/?serviceName=ecs

.. code-block:: console

   {
    "response": {
    "url": "https://calculator.otc-services.com/open-telekom-price-api/",
    "parameters": {
    "serviceName": "ecs"
    },
    "responseCode": 200,
    "code": "success",
    "message": "Success!",
    "stats": {
    ...
    },
    "result": {
    "services": {
    "ecs": [
    0: {
    "id": "OTC_S2M4_LI",
    "productName": "General Purpose 1:4 v2 s2.m.4 Linux",
    "opiFlavour": "s2.medium.4",
    "priceAmount": 0.046,
    ...
    },
    1: {
   ...
    },
    ...
    ]
    }
    }
    }
   }



5.4. Single service request with filtering
------------------------------------------

To load the data of an individual service, only the **serviceName** (see chapter 3. Parameters) of one
service (see chapter 4. Permitted values) must be specified.
In addition, those columns to which the filtering is to be applied must be specified via **filterBy**.


Request: https://calculator.otc-services.com/open-telekom-priceapi/?serviceName[0]=ecs& filterBy[opiFlavour][0]=s2.medium.4

.. code-block:: console

   {
    "response": {
    "url": "https://calculator.otc-services.com/open-telekom-price-api/",
    "parameters": {
    "serviceName": ["ecs"],
    "filterBy": {
    "opiFlavour": ["s2.medium.4"]
    }
    },
    ...
    "stats": {
    ...
    },
    "result": {
    "services": {
    "ecs": [
    0: {
    "id": "OTC_S2M4_LI",
    "productName": "General Purpose 1:4 v2 s2.m.4 Linux",
    "opiFlavour": "s2.medium.4",
    "priceAmount": 0.046,
    ...
    },
    1: {
    "id": "OTC_S2M4_OR",
    "productName": "General Purpose 1:4 v2 s2.m.4 Oracle",
    "opiFlavour": "s2.medium.4",
    "priceAmount": 0.077266,
    ...
    },
    ...
    ]
    }
    }
    }
   }


5.5. Single service request with filtering and selected fields
--------------------------------------------------------------

To load the data of an individual service, only the **serviceName** (see chapter 3. Parameters) of one
service (see chapter 4. Permitted values) must be specified.
In addition, those columns to which the filtering is to be applied must be specified via **filterBy**.
The required data must be transferred using the columns parameter, if you only want certain data to
be returned. The order of the fields within the request is relevant for the structure returned.



Request: https://calculator.otc-services.com/open-telekom-priceapi/?serviceName[0]=ecs&
filterBy[opiFlavour][0]=s2.medium.4&columns[]=productName&columns[]=id

.. code-block:: console

   {
    "response": {
    "url": "https://calculator.otc-services.com/open-telekom-price-api/",
    "parameters": {
    "serviceName": ["ecs"],
    "filterBy": {
    "opiFlavour": ["s2.medium.4"]
    }
    "columns": {
    0: "productName",
    1: "id"
    }
    },
    ...
    "result": {
    "services": {
    "ecs": [
    0: {
    "productName": "General Purpose 1:4 v2 s2.m.4 Linux",
    "id": "OTC_S2M4_LI"
    },
    1: {
    "productName": "General Purpose 1:4 v2 s2.m.4 Oracle",
    "id": "OTC_S2M4_OR"
    },
    ...
    ]
    }
    }
    }
   }




5.6.Multiple service request
----------------------------
To load the data of several services, they only have to be specified as an array in the **serviceName**.


Request: https://calculator.otc-services.com/open-telekom-priceapi/?serviceName[0]=ecs& serviceName[1]=obs

.. code-block:: console

   {
    "response": {
    "url": "https://calculator.otc-services.com/open-telekom-price-api/",
    "parameters": {
    "serviceName": ["ecs", "obs"]
    },
    ...
    "result": {
    "services": {
    "ecs": [
    ...
    ],
    "obs": [
    ...
    ]
    }
    }
    }
   }



5.7. Single record request
--------------------------
The data records can be filtered using the **filterBy** parameter, so that only one data record is
returned in the end. The product identifiers are unique across the services, so that you don't even
have to enter a service name here.

Request: https://calculator.otc-services.com/open-telekom-priceapi/?filterBy[id][0]=OTC_S2M4_LI

.. code-block:: console

   {
    "response": {
    "url": "https://calculator.otc-services.com/open-telekom-price-api/",
    "parameters": {
    "filterBy": {
    "id": ["OTC_S2M4_LI"]
    }
    },
    ...
    "result": {
    "services": {
    "ecs": {
    0: {
    "id": "OTC_S2M4_LI",
    "productName": "General Purpose 1:4 v2 s2.m.4 Linux",
    "opiFlavour": "s2.medium.4",
    ...
    }
    }
    }
    }
    }
