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

### Database
Data Tables vs Lookup Tables
Models generally contain two types of tables: `Data` (or `fact`) table, and `lookup` (or `dimension`) table\
**Data table**: contains numbers or values, typically at a granular level, with ID or "key" columns that can be used to create relationships\
**Lookup table**: descriptive, often test-based *attributes* about each dimension in a table.
