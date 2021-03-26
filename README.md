# pgtools
This package contains two functions that make it easier to fetch and store data from/to a Postgres database in R. 

## Installation
You can download and install the package from GitHub

```r
library(devtools)
install_github(repo = "guyschvitz/pgtools")
```

## pgViewTables: Search Tables in Database
This function offers an easy way look up all tables in the database by matching them to a search term (using PostgreSQL pattern matching). You can search either by schema name, table name or both. Leaving both the "table" and "schema" fields empty returns a dataframe of all tables in the database.

```r
library(pgtools)
library(RPostgres)

## Connect to DB
con <- dbConnect(Postgres(), host = MYHOST, user = MYUSER, 
                 dbname = MYDB, password = MYPASSWORD)

## Show all tables in schema maching "ras"
pgtools::pgViewTables(con, schema = "ras")

# table_schema   table_name
# 1  raster_tables hydepopc1880
# 2  raster_tables hydepopc1890
# 3  raster_tables hydepopc1900
# 4  raster_tables hydepopc1910
# 5  raster_tables hydepopc1920
# 6  raster_tables hydepopc1930
# 7  raster_tables hydepopc1940
# 8  raster_tables hydepopc1950
# 9  raster_tables hydepopc1960
# 10 raster_tables hydepopc1970
# 11 raster_tables hydepopc1980
# 12 raster_tables hydepopc1990
# 13 raster_tables hydepopc2000
# 14 raster_tables hydepopc2010

## Show all tables in schema matching "ras" with table names matching "19"
pgViewTables(con, schema = "ras", table = "19")

# table_schema   table_name
# 1  raster_tables hydepopc1900
# 2  raster_tables hydepopc1910
# 3  raster_tables hydepopc1920
# 4  raster_tables hydepopc1930
# 5  raster_tables hydepopc1940
# 6  raster_tables hydepopc1950
# 7  raster_tables hydepopc1960
# 8  raster_tables hydepopc1970
# 9  raster_tables hydepopc1980
# 10 raster_tables hydepopc1990
```

## pgRemoveLocks: Remove locks on tables in database
A common problem when trying to overwrite an existing PostgreSQL table is that the query hangs and never returns a result, which can cause your R session to crash entirely. This function finds and removes any locks in place on a given table so you can safely overwrite it.

```r
## Remove locks on all tables that match 'mytable'
pgRemoveLocks(con, 'mytable')
# [1] "No locks found"

## Overwrite 'mytable' with my.df
DBI::dbWriteTable(con, name = Id(schema = "myschema", table = "mytable"), my.df, overwrite=T)
```

