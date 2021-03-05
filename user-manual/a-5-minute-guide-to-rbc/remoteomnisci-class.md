# RemoteOmnisci class

## Connecting to OmniSciDB server

One can connect to the OmniSciDB server using the `RemoteOmnisci` remote class available in RBC:

```python
from rbc.omniscidb import RemoteOmnisci
omnisci = RemoteOmnisci(user='admin', password='HyperInteractive',
                        host='127.0.0.1', port=6274, dbname='omnisci')
```

## Storing data in a table

```python
table_name = 'my_table'
omnisci.sql_execute(f"""
    CREATE TABLE IF NOT EXISTS {table_name} (i4 INT, f8 DOUBLE)
""")
```

#### using SQL queries

```python

```

#### using load\_table\_columnar

```python
data = {
    'i4': [1, 2, 3],
    'f8': [1.0, 2.0, 3.0],
}

omnisci.load_table_columnar(table_name, **data)
_, r = omnisci.sql_execute(f"select * from {table_name}")
print(list(r))
# 
# 
```

## Defining functions

One can define UDF functions using `omnisci` as a decorator:

```python
@omnisci('int32(int32)')
def incr(i):
    return i + 1
```

```python
_, r = omnisci.sql_execute(f"select i4, incr(i4) from {table_name}")
print(list(r))
```

