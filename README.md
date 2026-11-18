# Library data pipeline – IB9SR0

student id - 5782076

## Generated data

| File | Rows | Columns |
|---|---|---|
| branches.csv | 8 | 4 |
| members.csv | 832 | 6 |
| items.csv | 1,200 | 6 |
| loans_2025_H1.csv | 2,824 | 8 |
| loans_2025_H2.csv | 3,236 | 8 |


## Bronze tables

| Table | Rows | Columns |
|---|---|---|
| bronze.branches | 8 | 6 |
| bronze.members | 832 | 8 |
| bronze.items | 1,200 | 8 |
| bronze.loans | 6,060 | 10 |

Bronze has the same rows as the generated files (loans = H1 + H2). Each table has 2 extra columns: `_source_file` and `_loaded_at`.
