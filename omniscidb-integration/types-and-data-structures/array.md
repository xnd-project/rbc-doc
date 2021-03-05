# Array

In OmniSciDB, an array has the following internal representation:

```c
typedef struct {
    T* data;  // contiguous memory block
    int64_t size;
    int8 is_null;  // boolean values in omniscidb are represented as int8 variables
} Array;
```

### Creating an Array programmatically

```python
from numba import types as nb_types
from rbc.omnisci_backend import Array

@omnisci('double[](int64)')
def create_array(size):
    array = Array(size, nb_types.double)
    for i in range(size):
        array[i] = nb_types.double(i)
    return array
```

\*Notice that returning an empty array is an invalid operation that might crash the server.

One can also use one of the array creation functions as specified in the [python Array API standard](https://data-apis.github.io/array-api/latest/API_specification/creation_functions.html): `full`, `full_like`, `zeros`, `zeros_like`, `ones` and `ones_like`:

```python
from numba import types as nb_types
import rbc.omnisci_backend as omni

@omnisci('double[](int64)')
def zero_array(size):
    return omni.zeros(size, nb_types.double)
```

### Accessing a pointer member

```python
@omnisci('double(double[], int64)')
def get_array_member(array, idx):
    return array[idx]
```

### Getting the size of an array

```python
@omnisci('int64(double[])')
def get_array_size(array):
    return len(array)
```

### Checking for null

You can either check if an array is null or if an array value is null

```python
@omnisci('int8(double[])')
def is_array_null(array):
    return array.is_null()

@omnisci('int8(double[], int64)')
def is_array_null(array, idx):
    return array.is_null(idx)
```

