# PBI DataDictionary
Come effettuare un Data Dictionary in un file PowerBI

```sql

DataDictionary=
VAR _Changes = DATATABLE(
    "Type", STRING,
    "Name", STRING,
    "Description", STRING,
    "Location", STRING,
    "Expression", STRING,
    {
        {"Changes","version 1.0","Importazione delle tabelle",BLANK(),"From Vittorio"}
       ,{"Changes","version 1.1","Impostazione Modello",BLANK(),"From Vittorio"}
    }
) 

VAR _Columns = SELECTCOLUMNS(
FILTER(
INFO.VIEW.COLUMNS()
,  [Table] <> "Data Dictionary" && NOT( [IsHidden])
)
, "Type" ,"Columns"
,"Name", [Name]
, "Description", [Description]
, "Location", [Table]
, "Expression", [Expression]
)

VAR _Measures = SELECTCOLUMNS(
FILTER(
INFO.VIEW.MEASURES()
,  [Table] <> "Data Dictionary" && NOT( [IsHidden])
)
, "Type" ,"Measures"
,"Name", [Name]
, "Description", [Description]
, "Location", [Table]
, "Expression", [Expression]
)

VAR _Tables = SELECTCOLUMNS(
FILTER(
INFO.VIEW.TABLES()
,  [Name] <> "Data Dictionary" &&  [Name]<> "Calculations" && NOT( [IsHidden])
)
, "Type" ,"Tables"
,"Name", [Name]
, "Description", [Description]
, "Location", BLANK()
, "Expression", [Expression]
)

VAR _Relationship = SELECTCOLUMNS(
INFO.VIEW.RELATIONSHIPS()
, "Type" ,"Relationships"
,"Name", [Relationship]
, "Description", BLANK()
, "Location", BLANK()
, "Expression", [Relationship]
)

RETURN UNION(_Changes, _Columns, _Measures, _Tables, _Relationship)
```