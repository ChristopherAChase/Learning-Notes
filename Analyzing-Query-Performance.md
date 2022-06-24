# Analyzing Query Performance for Developers 
[Pluralsign Course By Erin Stellato](https://app.pluralsight.com/library/courses/sqlserver-query-performance-developers/table-of-contents)
---

## Finding Information About Queries
SQL Server generates data about your queries every time you run a query
Some of the information we can find about query execution includes: 
- Query Text
- Query Plans
  - Tells us what SQL Server actually did 
- Execution Statistics

How to Get Some Information about your Query
- View Various Statistics using `SET STATISTICS ___ ON` syntax
  - `SET STATISTICS IO ON` provides details about the reads required from each table within your query
  - `SET STATISTICS TIME ON` provides information about how long it took to parse/compile and actually run your queries
  - `SET STATISTICS XML ON` provides an XML result set that when clicked, opens up a query plan viewer in SSMS
    - This is the same thing as clicking "Show actual Execution Plan" button in the UI.

You can also get the estimated query plan by highlighting the query, and then clicking the "Estimated Query Plan"
- You're asking SQL Server to analyze what it THINKS will happen when it actually runs the query. 

Can capture query data from extended events
- Good ones to utilize are
  - sql_statement_completed
  - sql_statement_completed
  - rpc_completed
  - sql_batch_completed

Query Plans and Query Text that have been stored by SQL Server can be accessed via Dynamic Management Objects (DMOs)
- `sys.dm_exec_sql_text({sql_handle | plan_handle})`
  - Allows you to view the individual sql query's text
- `sys.dm_exec_query_plan({plan_handle})`
- `sys.dm_exec_cached_plans`
- `sys.dm_exec_text_query_plan`

A query's aggregation statistics can be located in the `sys.dm_exec_query_stats` DMO
- Shows statistics such as
  - Number of times plan was used
  - Min and max logical reads when using the query plan.
  - Min and Max CPU usage when using the query plan.
  - etc.

As of 2016 we can also utilize the query store for looking at all of the above 
information for databases where query store is enabled. When enabled, query store
is a persisted storage of your query statistics. The aggregates can be configured to 
determine over what period the aggregates are stored/equated. 
- A [pluralsight course is available on this as well.](https://www.pluralsight.com/courses/sqlserver-query-store-introduction)


## Understanding Query Performance Metrics

There are a few query metrics that are of greater interest to us than others when looking at
individual query performance. 
- Duration
  - not the most reliable due to the fact that time can be affected by a lot of things, such as another query blocking the query you're running. This means that it's possible that there's nothing wrong with your query, even though it's taking more time. 
- CPU
  - Slightly better than duration. This allows us to see what the query needed in terms of CPU Cycles in order to execute. 
- IO
  - Can view both logical and physical IO needed. 
    - Physical IO Is more expensive
    - High Logical IO can mean a poorly structured schema or query
- Memory
  - May be cheap to purchase, but using a lot in a query is not desired. Maybe the query actually needs a lot of memory, but maybe SQL server is poorly estimating the required amount of memory. 

## Reading Query Plans

Every query has a plan generated

- Optimizer
  - Somewhat of a framework within SQL Server that is responsible for finding a query plan
  - Starts with a parsing phase that makes sure the syntax is correct, and then generates a parser tree.
    - This parser tree is somewhat an internal representation of the query.
  - After the parsing phase, the optimizer moves to the binding phase, where the optimizer validates that all the objects involved exist, and that they're accessible to the user (through security checks)
  - The output of this binding phase is the query tree
    - The query tree has all of the logical operations in it, which describe all of the actions that will occur during the processing of the query - such as reading from a table, sorting data, performing a join, etc. 
  - This query tree is then fed to the optmization phase, which uses transformation roles to generate physical operations from the logical ones. 
    - Reading data from a table (Logical) -> Index Seek (Physical)
    - Performing a join (Logical) -> Nested Loop (Physical)
  - The physical operations are then turned into the query plan

When looking at the query plan, each step or icon, represents a physical operation. 
- Each physical operation is linked by arrows that represent the flow of the query plan. 
- The Cost underneath each operation represents the estimated cost of each operation in relation to the cost of the entire query (a cost of 10% means that operation was roughly 10% of the entire query). 
  - This cost is **NOT** a representation of time, or CPU. It is a unit-less measure that is simply relative and comparitive to the other query operators, and costs are **ALWAYS** an estimate, there is never an actual, calculated value of cost.
- Each operation also has a cost in the properties of the operation. It has a CPU cost, and an IO cost.
  - These values are also estimates, but calculated through various algorithms.
  - The cost is something that SQL Server generates based off what it knows about your data (the statistics of your data)
    - The more rows SQL Server thinks it's going to have to read through, the higher the CPU cost will be
    - The more pages SQL Server thinks it's going to have to read through, the higher the IO cost will be. 

When reading a query plan, there is a few important things to note:
- When reading a plan, we read it from right to left. This is because this is the flow of the data. 
  - The data starts from the tables, then the data pulled from the tables gets moved into a join operator, and the data after the join operator moves to the select statement.
- We can also read a plan from left to right, but reading it in this direction is representative of the flow of control. 
  - Starting at the root operator (SELECT), the SELECT operator controls our query. The select feeds over to the Nested loop (join), which then pulls data from the two tables it is joining. 
- Right to Left = Flow of data
- Left to Right = Flow of Control
- Need to read the plan in both directions

A plan can be viewed in both the UI, through SSMS, or it can be viewed in the XML format (through one of the DMOs, or right clicking the visual plan, and selecting view XML). This XML structure changes on each release, because they add new information, each iteration. But the structure is a strict schema, and you can view that XML Schema here: 
- [http://schemas.microsoft.com/sqlserver/2004/07/showplan/](http://schemas.microsoft.com/sqlserver/2004/07/showplan/)
  - From here, you'll just select the year of SQL Server you have/are working with. 






















