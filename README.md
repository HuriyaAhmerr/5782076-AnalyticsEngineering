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


## Silver tables

Silver contains the cleaned tables. `_source_file` is kept so each row can be traced back to its file; `_loaded_at` is dropped as silver is fully replaced on each run.

| Table | Rows | Columns | Column names |
|---|---|---|---|
| silver.branches | 8 | 5 | branch_id, branch_name, area, opened_year, _source_file |
| silver.members | 800 | 7 | member_id, join_date, age_band, membership_type, home_branch_id, postcode, _source_file |
| silver.items | 1,200 | 7 | item_id, title, item_type, category, replacement_cost_gbp, home_branch_id, _source_file |
| silver.loans | 5,898 | 14 | loan_id, member_id, item_id, branch_id, loan_date, due_date, return_date, fine_gbp, is_returned, days_late, is_late, fine_waived, loan_before_join, _source_file |



## Business problem

The library wants to understand where and when its items are borrowed, and where items are coming back late or not at all, so it can plan staffing and reduce lost or unavailable stock.

| # | Business question | Why it matters |
|---|---|---|
| Q1 | Which branches are the busiest? | Staffing and resources should go where demand is highest |
| Q2 | How does demand change through the year? | Busy and quiet months help plan staff holidays and events |
| Q3 | How often are items returned late, and is it worse at some branches or for some item types? | Late items are not available for other members to borrow |
| Q4 | How many items are still not returned, and where? | Unreturned items are lost stock and may need to be chased or replaced |

## Gold tables

Each gold table is built from the cleaned silver tables and exported as a CSV file for the Tableau dashboard. Every dashboard figure comes directly from a gold table and column.

| Gold table | One row per | Answers |
|---|---|---|
| `gold.loans_by_branch` | branch | Q1, Q3, Q4 |
| `gold.loans_by_month` | month | Q2 |
| `gold.loans_by_item_type` | item type | Q3, Q4 |

- **Late return share** = late returns ÷ returned loans
- **Not returned share** = loans not returned ÷ all loans
