# Integrate with OmniSciDB

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
    'i4': [0, 1, 2, 3, 4],
    'f8': [0.0, 1.0, 2.0, 3.0, 4.0],
}

omnisci.load_table_columnar(table_name, **data)
_, r = omnisci.sql_execute(f"select * from {table_name}")

for i4, f8 in list(r):
    print(f"{i4=}, {f8=}')

# i4=0, f8=0.0
# i4=1, f8=1.0
# i4=2, f8=2.0
# i4=3, f8=3.0
# i4=4, f8=4.0
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

for i4, incr in list(r):
    print(f"{i4=}, {incr=}')

# i4=0, incr=1
# i4=1, incr=2
# i4=2, incr=3
# i4=3, incr=4
# i4=4, incr=5
```

