# Supported Types and Data Structures

OmniSciDB supports many [data types](https://docs-new.omnisci.com/sql/data-definition-ddl/datatypes-and-fixed-encoding) but not all of them are supported by the Remote Backend Compiler. 

### Scalar types

| Datatype | Size \(bytes\) | Notes |
| :--- | :--- | :--- |
| BOOLEAN | 1 | TRUE: `'true'`, `'1'`, `'t'`. FALSE: `'false'`, `'0'`, `'f'`. Text values are not case-sensitive. |
| TINYINT | 1 | Minimum value: `-127`; maximum value: `127` |
| SMALLINT | 2 | Minimum value: `-32,767`; maximum value: `32,767` |
| INT | 4 | Minimum value: `-2,147,483,647`; maximum value: `2,147,483,647`. |
| BIGINT | 8 | Minimum value: `-9,223,372,036,854,775,807`; maximum value: `9,223,372,036,854,775,807`. |

### Array

RBC also supports `Array<T>` where `T` is one of the following scalar types seen above. Both fixed and variable length arrays are supported in RBC. For more information, see the Array page.

### Column

### ColumnList

### Cursor



