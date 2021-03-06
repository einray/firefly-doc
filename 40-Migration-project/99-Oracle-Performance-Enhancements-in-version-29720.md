In  version 29720 we implemented some performance related oracle tuning strategies that we've been using succesfully for a while now with MSSQL . Our internal testing showed that they'll translate to a significant improvement in Oracle based applications.

We've enabled these enhancements by default, but since it's an early release, we allow disabling them via ini settings.

We ask that you try these features and if you run into a performance issue, you can disable them using the ini - but please let us know so that we can investigate the cause of the problem you ran into and improve these enhancements.

# First Rows
According to Oracle documentation, when you issue a select statement Oracle assumes that you would like to recieve all the rows.

Oracle expects you to let it know if you are only looking for the first rows by adding the `FIRST_ROWS` hint.

## UIControllers
In a UIController (online tasks in magic) you would like for the first rows to fill the grid fast, while the other rows will load later.
So from this verison, queries that are issued for UIControllers' From entity will include the `FIRST_ROWS` hint.

You can disable it by setting the `DisableUIControllerHint = Y` in the `ORACLE_TEST` section. 

You can do that in the command line like this:
```
/[ORACLE_TEST]DisableUIControllerHint=Y
```

## Relations
When performing a relation (link in magic), that is not based on a unique filter, you only want the first row. In this version we added the `FIRST_ROWS` hint for such cases.

You can disable it by setting the `DisableRelationHint = Y` in the `ORACLE_TEST` section. 

You can do that in the command line like this:
```
/[ORACLE_TEST]DisableRelationHint=Y
```



# Improve string data types
In previous versions, all oracle statement bound parameters used `Char` instead of `Varchar2`. This was done due to a bug in early Oracle .NET clients.

In this version the statement is sendt using `Char` and `Varchar2` according to the field's storage.

You can disable it by setting the `AlwaysUseChars = Y` in the `ORACLE_TEST` section. 

You can do that in the command line like this:
```
/[ORACLE_TEST]AlwaysUseChars=Y
```

# Improve Date data types
In previous version bound paramers that related to dates were sent as `timestamp` due a to a .NET Default. From this version `Date` will be used instead.

