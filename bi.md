## Intro
### Transform vs Add Column
Transform: modifing from the existing column, overriding the selected column\
Add Column: add an additional column
### Ways to change the start date of the week
```
= Table.AddColumn(#"Inserted Day Name", "Start of Week", each Date.StartOfWeek([Date],1), type date)
= Table.AddColumn(#"Inserted Day Name", "Start of Week", each Date.StartOfWeek([Date],Day.Monday), type date)
```
### Rolling table - useful tool
Start from a blank query   `=#date(2018,1,1)`\
Literal which will be the starting date\
`= List.Dates(Source, Number.From(DateTime.LocalNow())-Number.From(Source),#duration(1,0,0,0))`\
Calculate the current date, compare it to the literal we created. Based on the difference between those two dates, list out all the individual values where the duration is 1 day.
### Poviting vs transpose
Poviting: turning distinct row values into columns\
Transpose: similar, but doesn't recognises unique values, but instead, transform the entire table
### Merge queries
Join tables based on a common column (like vlookup) - add columns to an existing table\
**IMPORTANT NOTES:**
Just because you can merge the tbales doesn't mean you should. In general, it's better to keep the tables separate and define relationships between them.
### Appending queries
Combine (or stack) tables that share the exact same column structure and data types - appending adds rows to an existing table\
Downside: if tried to delete the old table, it won't allow you because the new appended table requires the old table to reference.\
--> use the `folder` approach
### Refreshing queries
By default, will refresh all queries in the model\
Can uncheck the `include in report refresh` to exclude individual queries from refresh --> useful to exclude queries that don't change often, like lookups or static data tables

## Database
### Data Tables vs Lookup Tables
Models generally contain two types of tables: `Data` (or `fact`) table, and `lookup` (or `dimension`) table\
**Data table**: contains numbers or values, typically at a granular level, with ID or "key" columns that can be used to create relationships\
**Lookup table**: descriptive, often test-based *attributes* about each dimension in a table (rich info like model, colour ...).
### Relationships vs Merged tables
Issue with **merging queries* or use **LOOKUP/RELATED**:\
In excel, maybe okay because don't have the tools to integrate the data. However, create too much redundant data, use up memory and processing power. Simply too much information in a single tabel.
### Snowflakes schemas
Models with a chain of dimension tables are often called **Snowflake** schemas (whereas **star** schemas usually have individual lookup tables surrounding a central data table.
### Avtive vs inactive relationships
Solid line -> active relationship, doted line -> inactive relationship.\
Need to deactive first and then, active the inactive relationship --> one at a time.
### Cardinality
The *uniqueness* of values in ac cloumn\
**one-to-many** cardinality, **one** instance of each primary key, potentially **many** instances of each foreign key.\
one-to-one: inefficient so could merge two tables into one (still respects the normalisation rules)\
many-to-many: unable to identify the relationship\
On the diagram, `1` refers to the primary key (*one side*), `*` refers to the foreign key (*many side*)
### Filter flow
Filter direction (arrow) in each relationship, by default, will point from the `one` side (lookups) to the `many` side (data)\
Filters **cannot** flow upstream., So use the Primarykey from the lookup table then can see data from other tables.

### Two-way Filter FLow 
Bi-directional filter can have the data going both downstream and upstream.\
However, if the key doesn't exist in one table, it will not pass onto the other table --> see screen shot\
It is dangerous, so only use **when necessary**: risk of creating **ambiguous relationships**\
Can only have one bi-direction filter at a time or it will show error meessage. --> otherwise it will pass multiple conflicting filters to the same lookup table.\
**Way to prevent filtering issues:**\
Use `Hide in report view` to hide the foreign in the tables.
### Calculated columns vs measure
|**Calculated Columns**|**Measures**|
|-|-|
|Values are calculated based on info from eacho row or table (has row context)|Based on info from any filters in the report (hasfilter context)|
|Appends static values to each row in a table and stores themm in the model (increases file size)|Doesn't create new data in the tables|
|Recalculate on data source refresh or when changes ar made to component columns|Recallculate in response to any change  to fiilters within the report|
|Primarily used as **rows, columns, slicers,filters**|Almost *always* used within the **values** field of a visual|
|Live in TABLES|Live in VISUALS|
### Implicit vs Explicit measures
**Implicit measures**: created when you drag raw numerical fields (like OrderQuantity) into the value pane od of a visual and manually select the aggregation mode.\
--> can only referenced within the specific visualisation\
**Explicit measures**: created by actually entering DAX funcitons to define calculated columns or measures.\
--> can be referenced in other tables. 
### DAX syntax & operators
*+ - * / ^*\
*= > < >= <= <> (not equal to)*\

|Text/logical operator|Meaning|example|
|-|-|-|
|**&**|Concatenates two values to produce one text string|[City] & " " & [State]|
|**&&**|Cret an AND condition between two logical expressions|([State]='MA') && {[Quantity]>10}|
|\|\||Create an OR condition between two logical expressions|([State]='MA') **||** {[Quantity]>10}|
|**IN**|Create a logical OR condition based on a given list (using curly brackets)|'Store Lookup'[State] IN {"MA","CT","NY"}|
