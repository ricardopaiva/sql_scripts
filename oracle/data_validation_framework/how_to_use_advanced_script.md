# How to Use the Advanced Data Validation Scripts in Oracle
[![License: CC0](https://img.shields.io/badge/License-CC0-red)](LICENSE "Creative Commons Zero License by DataResearchLabs (effectively = Public Domain")
[![YouTube](https://img.shields.io/badge/YouTube-DataResearchLabs-brightgreen)](http://www.DataResearchLabs.com)
---

### Overview
This advanced data validation scripts runs the exact same 66 test cases (T001-T066) as the basic script.  
However, this pair of scripts is more sophisticated for the follow reasons:
1. Output is written to a "temp" table during the test execution phase, then returned as a single SELECT "report"
2. There are two scripts, one to generate the "temp" tables and one to execute the test cases
3. The output is more comprehensive than just test id, status, and description.  The output also includes detailed rejection codes, expected results, and actual results behind every fail.
<br><br>

### Step 1 - Download & Install Oracle SQL Developer
<details><summary>Expand if you want to download and install Oracle SQL Developer...</summary>
   
1. Oracle provides a powerful SQL editor named "Oracle SQL Developer" for free download and use.  
2. If it is not already installed on your machine (and you're not using another database IDE like Toad), then download from <b>[here](https://www.oracle.com/tools/downloads/sqldev-downloads.html)</b> and install, following the prompts.
</details>
<br>

### Step 2 - Download & Deploy the Demo Data
<details><summary>Expand if you want to download and deploy the "demo_hr" test dataset...</summary>

If you'd like to run the test script as-is first, before copy-pasting the concepts out and applying to yuor own databases, then you will need to download and deploy the demo_hr test dataset.
1. Download the "demo_hr" schema / table definitions from <b>[here](https://raw.githubusercontent.com/DataResearchLabs/sql_scripts/main/oracle/data_validation_framework/demo_data/demo_hr_01_create_tables.sql)</b>.
2. Run the script on an Oracle server and database where you have permissions (local is fine too).
3. Download the "demo_hr" test data population script from <b>[here](https://raw.githubusercontent.com/DataResearchLabs/sql_scripts/main/oracle/data_validation_framework/demo_data/demo_hr_02_populate_tables.sql)</b>.
4. Run the script on the same Oracle server and database.
5. Using Oracle SQL Developer (or equivalent SQL IDE), confirm that the tables exist and the data is populated.
</details>
<br>

### Step 3 - Download & Configure the Advanced Data Validation Script
<details><summary>Expand if you need instructions on how to download and configure the basic script...</summary>
   
1. Download the basic validation script from <b>[here](https://raw.githubusercontent.com/DataResearchLabs/sql_scripts/main/oracle/data_validation_framework/sql_scripts/dvf_basic_script.sql)</b>.
2. There are **no** configuration changes needed for the script, it will work out of the box against the demo_hr schema and data created in Step #2 above.
3. Pick an appropriate directory in which to save the script.  Open your SQL Editor pointing to the appropriate Oracle Server and demo_hr schema.
</details>
<br>

### Step 4 - Review the Basic Data Validation Script
<details><summary>**Expand if you would like to see a review of the script layout and what each data validation test case looks like ...></summary>

The script currently consists of 1,064 lines of SQL code broken down as follows:
* Lines 1-44 are the comment block header, containing notes and definitions
* Lines 45-1,064 are the 66 individual example validation test cases (written as SQL SELECTs)

A typical data validation test has SQL code that looks something like this: <br>  

<img src="https://github.com/DataResearchLabs/sql_scripts/blob/main/img/04_data_val_oracle_example_test_case_sql_code.png">

This test case validates that no carriage return (CR) or line feed (LF) characters exist in the last_name column across all rows. 

Notice the following aspects of the SQL code:
1. Each data validation test case is written as one or more SQL SELECT statements.

2. There is one (or more) **inner queries**  (lines 453-459 above)
    * These return many detail rows with business validation logic applied.  
    * The columns returned vary by validation test case, but typically have a primary key or unique key value returned so you can easily identify which row faile
    * There is also always a status field returned with a unique rejection code (eg: REJ-01 above) with the expected result (no CR or LFs), and the actual result including the position of the bad character in the source field.
    * Note that you can highlight and run just the inner query SELECT(s) to see all relevant rows with specific failure details    

3. There is one **outer query** (lines 449-452 and 461-462)
    * It rolls all the detail rows up to a single summary row with pass or fail judgment.
    * It returns column **tst_id** - the test ID (hard-coded when write script)
    * It returns column **status** - the test result (re-calculated with every test run).  Usually "P" for pass or "FAIL"...or add your own such as "WARN", "SKIP", or "BLOCK"
    * It returns column **tst_dscr** - the data validation test description (hard-coded when write script)
</details>
<br>

### Setp 5 - Execute the Basic Data Validation Script
Here are the steps to execute the basic script in Oracle SQL Developer (typical output show in the screenshot below).  
1. Open Oracle SQL Developer (or equivalent SQL Editor)
2. Blue Dot #1 - You must load the basic validation script into SQL Developer (or equivalent IDE)
3. Blue Dot #2 - Be sure to click the "Run script" button (or equivalent in other IDEs) so that all test cases will output to a single text document on screen (**not** as 66 separate grids)
4. Blue Dot #3 - The output is concisely laid out for all data validation test cases.  The red-boxed test case includes test_id (eg: T001) in column #1, followed by the status (eg: pass or fail) in column #2, and finally ends with the test description on the right in column #3 (because width varies so much want it on the end for better readability).
<img src="https://github.com/DataResearchLabs/sql_scripts/blob/main/img/05_data_val_oracle_run_results1.png">