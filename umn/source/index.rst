================================================
Welcome to the documentation of Pricing-API
================================================


1. Stats
=======================
Document status: 2023-01-24

Version: 0.5

End points:

https://calculator.otc-services.com/en/open-telekom-price-api/  

https://calculator.otc-services.com/en/open-telekom-price-api-ui/ 

https://calculator.otc-services.com/de/open-telekom-price-api/    

https://calculator.otc-services.com/de/open-telekom-price-api-ui/ 



2. Parameters
=======================

The values can be sent via POST or GET.

At first a checksum is formed from the parameters, which is also used in parts as a key for the cache
entry. So the cached data is returned, if there is a valid cache entry. Extensive validation of the
entries can then be omitted.

All entries are validated, filtered and grouped by service, if no cache entry for the respective request
was found. A pagination structure is also supplied for very large amounts of data.

2.1.Validation
---------------------------
• Numerical values are always cast to an <Int>
• The values of responseFormat, serviceName, groupBy, region and columns as well as the keys of filterBy are compared with the stored identifiers, settings or with the field names in the database schema. This eliminates any values that are beyond the technical realities.
• Only the value of filterBy must be filtered and escaped



2.2.Overview
-------------------------


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
