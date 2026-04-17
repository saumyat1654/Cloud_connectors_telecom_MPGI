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

Step 1: Data Cleanup (3 Separate Mappings for the 3 source files and then load them into database for further processing)
1. Expression(handle null values, standardize date/time)
2. Aggregator(sort, distinct)
The 3 mapping task made here are- MCT_M_TOWER_MASTER_TELECOM, MCT_CUSTOMER_MASTER_TELECOM, MCT_CDR_STAGE_LOADING.


Step 2: Stage Table load (source- CDR_RAW_DATA)
1. Two parallel lookups
2. Expression(calculate revenue and isinternational)
Mapping task- MCT_CDR_TRANSFORMED_DATA

Step 3: High usage (source - CDR_FINAL)
1. Aggregator(group by on caller, customer name, calculate total duration)
2. Filter(total duration>200)

Step-3 : Taskflow (TF_TELECOM)
1. first run MCT_M_TOWER_MASTER_TELECOM, MCT_CUSTOMER_MASTER_TELECOM in parallel, since they don't depend on each other.
2. Once it's successfully executed, then run MCT_CDR_STAGE_LOADING.
3. Now we have to put decision here. If the previous task status is success then execute MCT_CDR_TRANSFORMED_DATA, followed by MCT_HIGH_USAGE and then Email notification that the call duration is greater than the threshold.
4. If the task status is not success, then send a email notification to the developer, and use a command task to create a audit log file for further evaluation.

KPIs-
1. Daily call summary (MCT_M_KPI_DAILY_CALLING)
   Used aggregator( Grouped data by date)
   1. SUM(DURATION) → Total Duration
   2.  AVG(DURATION) → Average Duration
   3.  COUNT(CALLID) → Total Calls
      
2. Call type performance analysis (M_KPI_CALL_TYPE)
  1. used expression and then aggregator
  2.  group by applied on call type
  3.  calculated total calls, total duration, revenue, average duration

3. International call monitoring (M_KPI_INTERNATIONAL_MONITORING)
  First used aggregator and applied group by on call type.
  then calculated -
  •	International Call Count (count(callid))
  •	International Call Duration (sum(duration))
  •	International Revenue (sum(revenue))
  Then used expression to calculate % of international calls ((international_call_count * 100) / total_call_count)).

4. Revenue Data (MCT_KPI_REVENUE)
  Used-
 1. Aggregator(revenue per customer)
 2. expression(calculate % contribution to total revenue)
 3. sorter(sort in descending order)
 4. rank(top 2 customers)
    
  
