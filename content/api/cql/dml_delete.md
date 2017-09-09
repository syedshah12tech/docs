---
title: DELETE
summary: Deletes rows from a table.
---
<style>
table {
  float: left;
}
#ptodo {
  color: red
}
</style>

## Synopsis
`DELETE` removes rows from a specified table that meet a given condition. Currently, YugaByte can deletes one row at a time. Deleting multiple rows is not yet supported. For example, the following delete command deletes a row of the table `yugatab` that have `id = 7`.
<p id=psyn>`DELETE FROM yugatab WHERE id = 7;`</p>

## Syntax
```
delete ::= DELETE FROM table_name
               WHERE where_expression
               [ IF { [ NOT ] EXISTS | if_expression } ];
```
where
  <li>`table_name` is an identifier.</li>
  <li>See [Expression](..#expressions) for more information on syntax rules.</li>
  <li>See Semantics Section for restrictions on `where_expression` and `if_expression`.</li>

## Semantics
<li>An error is raised if the specified `table_name` does not exist.</li>
<li>The `where_expression` and `if_expression` must be resulted in boolean values.</li>
<li>The `where_expression` must specify conditions for all primary-key columns.</li>
<li>The `where_expression` must not specifiy conditions for any regular columns.</li>
<li>The `where_expression` can only apply `AND` and `=` operators. Other operators are not yet supported.</li>
<li>The `if_expression` can only apply to non-key columns (regular columns).</li>
<li>The `if_expression` can contain any logical and boolean operators.</li>
<li>Deleting a column from a table is not yet supported.</li>

## Examples

``` sql
cqlsh:example> CREATE TABLE employees(department_id INT, 
                                      employee_id INT, 
                                      name TEXT, 
                                      PRIMARY KEY(department_id, employee_id));
cqlsh:example> INSERT INTO employees(department_id, employee_id, name) values (1, 1, 'John');
cqlsh:example> INSERT INTO employees(department_id, employee_id, name) values (1, 2, 'Jane');
cqlsh:example> INSERT INTO employees(department_id, employee_id, name) values (2, 1, 'Joe');
cqlsh:example> SELECT * FROM employees;

 department_id | employee_id | name
---------------+-------------+------
             2 |           1 |  Joe
             1 |           1 | John
             1 |           2 | Jane
cqlsh:example> -- Delete statements identify rows by the primary key columns.
cqlsh:example> DELETE FROM employees WHERE department_id = 1 AND employee_id = 1;
cqlsh:example> -- Deletes on non-existent rows are no-ops.
cqlsh:example> DELETE FROM employees WHERE department_id = 3 AND employee_id = 1;
cqlsh:example> SELECT * FROM employees;

 department_id | employee_id | name
---------------+-------------+------
             2 |           1 |  Joe
             1 |           2 | Jane
```

## See Also

[`CREATE TABLE`](../ddl_create_table)
[`INSERT`](../dml_insert)
[`SELECT`](../dml_select)
[`UPDATE`](../dml_update)
[`Expression`](..#expressions)
[Other SQL Statements](..)