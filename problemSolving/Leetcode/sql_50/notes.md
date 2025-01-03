- **COALESCE(Result2.attended_exams, 0)**

  ```sql
  COALESCE(Result2.attended_exams, 0) AS attended_exams  # Step 3: Use COALESCE to ensure 0 is shown if no matching record in Examinations
  ```

- **conditional expression**

  ```sql
  CASE WHEN activity_type = 'end' THEN timestamp ELSE NULL END
  ```

- **DATE_FORMAT**

  - **What it does**:
    - Formats a date into a specific format as per your requirements.
  - **Example**:
    ```sql
    SELECT DATE_FORMAT(NOW(), '%Y-%m-%d') AS FormattedDate;
    ```
    - `%Y`: Year in 4 digits (e.g., `2024`).
    - `%m`: Month in 2 digits (e.g., `12`).
    - `%d`: Day in 2 digits (e.g., `15`).
  - **Use Case**: To display dates in a user-friendly or consistent format.

- **LEFT(trans_date, 7)**

  - **What it does**:
    - Extracts the **first 7 characters** from the string `trans_date`.
  - **Example**:
    ```sql
    SELECT LEFT('2024-12-15', 7) AS YearMonth;
    ```
    - **Result**: `2024-12` (Year and month only).
  - **Use Case**: To get the year and month from a `DATE` or `DATETIME` string.

- **DATE_SUB('2019-07-27', INTERVAL 30 DAY)**

  - **What it does**:
    - Subtracts 30 days from the given date (`2019-07-27`).
  - **Example**:
    ```sql
    SELECT DATE_SUB('2019-07-27', INTERVAL 30 DAY) AS NewDate;
    ```
    - **Result**: `2019-06-27`.
  - **Use Case**: To calculate a date that is a specific number of days before a given date.

- **Difference Between Where and Having**

  - **`WHERE`** is for filtering rows before aggregation.
  - **`HAVING`** is for filtering groups after aggregation.

- GROUP_CONCAT(DISTINCT product ORDER BY product ASC) AS products
