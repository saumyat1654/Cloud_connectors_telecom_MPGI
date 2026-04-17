# Cloud_connectors_telecom_MPGI
This project demonstrates an end-to-end ETL pipeline built using Informatica Intelligent Cloud Services (IICS) for processing Telecom Call Detail Records (CDR).
Telecom companies generate massive volumes of call data daily. This project automates the process of:
1. Extracting raw CDR data from CSV files
2. Transforming and cleaning the data
3. Calculating business metrics like revenue
4. Loading processed data into a database

Objective-
To design and implement a scalable ETL solution that:
1. Processes daily telecom call data
2. Ensures data quality and consistency
3. Generates insights for business analysis

The source file that we have are-
1. CDR_RAW_Telecom.csv
2. Customer_Master_Telecom.csv
3. Tower_Master_Telecom.csv

Step 1: Data Cleanup (3 Separate Mappings)
* Expression(handle null values, standardize dae/time)
* Aggregator(sort, distinct)

Step 2: Stage Tbale load
1. Two parallel lookups
2. Expression(calculate revenue and isinternational)

Step-3 : 
Then we created taskflow and run the two mapping in parellel and if it success then run third mapping.
After that put decision and is the status is success then run the stage table load mapping.
If it fails then, then send the email notification in that to developer.
Also created the high usage mapping which shows the high usage of call duration.

KPIs-
1. Daily call summary
   Used aggregator
  * SUM(DURATION) → Total Duration
  * AVG(DURATION) → Average Duration
  *  COUNT(CALLID) → Total Calls
  * Grouped data by date
2. Call type performance analysis
  used expression and then aggregator
  group by applied on call type
  calculated total calls, total duration, revenue, average duration
3. International call monitoring
  First used aggregator and applied group by on call type.
  then calculated -
  •	International Call Count
  •	International Call Duration
  •	International Revenue
  Then used expression to calculate % of international calls.
4. Revenue Data
  Used-
 1. Aggregator(revenue per customer)
 2. expression(calculate % contribution to total revenue)
 3. sorter(sort in descending order)
 4. rank(top 2 customers)
    
  
