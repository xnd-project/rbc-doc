# Simple example \(OmniSciDB\)

The RBC package implements support for defining and registering user-defined functions for the OmniSciDB SQL engine. The following types of user-defined functions are supported:

* UDFs that are applied to a DB table row-wise,
* UDTFs \(table functions\) that are applied to the DB table columns.

In the following, we explain how to use the RBC Python package `rbc` for connecting to an OmniSciDB server, defining custom UDFs and a UDTF, registering these to the OmniSciDB server, and finally, how to use the user-defined functions in a SQL query using `rbc` tools as an example.

### Connecting to OmniSciDB server

Assuming that an OmniSciDB server is running, registering a new user-defined function requires establishing a connection to the server. This can be done directly:

```python
from rbc.omniscidb import RemoteOmnisci
omnisci = RemoteOmnisci(user='admin', password='HyperInteractive',
                        host='127.0.0.1', port=6274, dbname='omnisci')
```

or using an existing connection session id \(not implemented, see [rbc issue 180](https://github.com/xnd-project/rbc/issues/180)\):

```python
omnisci = RemoteOmnisci(connection=con)
```

For the sake of having a complete example, let's create a sample table:

```python
omnisci.sql_execute('drop table if exists simple_table')
omnisci.sql_execute('create table if not exists simple_table (x FLOAT, i INT);');
omnisci.load_table_columnar('simple_table',
                            x = [1.1, 1.2, 1.3, 1.4, 1.5],
                            i = [0, 1, 2, 3, 4])
```

### Defining UDFs using Python functions

We create two new UDFs that increment row values by 1, one UDF for FLOAT columns and another for INT columns:

```python
@omnisci('float(float)', 'int(int)')
def myincr(x):
    return x + 1
```

Notice that the two UDFs can be defined as a single Python function `myincr` because RBC/OmniSciDB supports overloading user-defined function names.

### Registering UDFs - row-wise function

To register these UDFs to OmnisciDB, one can call

```python
omnisci.register()
```

but when using the `omnisci` object for making SQL queries then the registration of any new UDFs is triggered **automatically**.

That's it! Now anyone connected to the OmniSciDB server can use the SQL function `myincr` in their queries.

### SQL query using omnisci.sql\_execute

For example, one can use the RBC provided `omnisci` object to send queries:

```python
descr, result = omnisci.sql_execute('SELECT x, myincr(x) FROM simple_table')
for x, x1 in result:
    print(f'x={x:.4}, x1={x1:.4}')
```

that will output:

```text
x=1.1, x1=2.1
x=1.2, x1=2.2
x=1.3, x1=2.3
x=1.4, x1=2.4
x=1.5, x1=2.5
```

### Defining UDTFs - table functions

Table functions act on database table columns and their results are stored in so-called output columns of temporary tables. Let's implement a new SQL table function that computes a new table with all columns incremented by user-specified value:

```python
@omnisci('int(Cursor<float>, float, RowMultiplier, OutputColumn<float>)')
def incrby(x, dx, m, y):
    for i in range(len(x)):
        y[i] = x[i] + dx
    return len(x)

omnisci.register()
```

Before trying it out, let's explain some of the details here:

* **Return value** - ****The return value of a UDTF definition defines the length of the output columns. The output columns arguments memory is pre-allocated for the size `m * len(x)` where column sizer parameter `m` is a literal constant specified by the user in a SQL query and `len(x)` represents the size of the first input column. In case the UDTF definition returns the output column size value smaller than `m * len(x)`, the memory of output columns will be re-allocated accordingly. The return type of a UDTF definition must be 32-bit integer and the type of column sizer parameters can be `RowMultiplier`, `ConstantParameter`, or `Constant`.
* **Cursor** - The `Cursor<...>` represents the cursor over input table columns. For instance, `Cursor<float, int>` would correspond to two arguments of the UDTF definition, one being the input column containing float values and another being input column containing int values.

One can call the new table function to increment the FLOAT column `x` by value `2.3` from a SQL query as follows:

```python
descr, result = omnisci.sql_execute('''
  SELECT * FROM TABLE(INCRBY(CURSOR(SELECT x FROM simple_table),
                             CAST(2.3 AS FLOAT), 1))
''')
for y, in result:
    print(f'y={y:.4}')
```

that will output

```text
y=3.4
y=3.5
y=3.6
y=3.7
y=3.8
```

