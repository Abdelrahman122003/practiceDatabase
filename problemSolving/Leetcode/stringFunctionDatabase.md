String functions in databases allow you to manipulate, extract, and analyze string data. These functions are commonly used in SQL queries for tasks like formatting, searching, replacing, and comparing strings. Below are some widely used string functions with examples:

### **1. `LENGTH`**

- Returns the length of a string.
- **Syntax:** `LENGTH(string)`
- **Example:**
  ```sql
  SELECT LENGTH('Hello World'); -- Output: 11
  ```

---

### **2. `UPPER` / `LOWER`**

- Converts a string to uppercase or lowercase.
- **Syntax:**
  - `UPPER(string)`
  - `LOWER(string)`
- **Example:**
  ```sql
  SELECT UPPER('hello'), LOWER('WORLD'); -- Output: 'HELLO', 'world'
  ```

---

### **3. `SUBSTRING` / `SUBSTR`**

- Extracts a portion of a string.
- **Syntax:**
  - `SUBSTRING(string FROM start FOR length)` (ANSI SQL)
  - `SUBSTR(string, start, length)` (MySQL, Oracle)
- **Example:**
  ```sql
  SELECT SUBSTRING('Hello World', 1, 5); -- Output: 'Hello'
  ```

---

### **4. `CONCAT`**

- Concatenates two or more strings.
- **Syntax:** `CONCAT(string1, string2, ...)`
- **Example:**
  ```sql
  SELECT CONCAT('Hello', ' ', 'World'); -- Output: 'Hello World'
  ```

---

### **5. `TRIM`**

- Removes leading and trailing spaces (or specific characters).
- **Syntax:** `TRIM([characters] FROM string)`
- **Example:**
  ```sql
  SELECT TRIM('   Hello   '); -- Output: 'Hello'
  ```

---

### **6. `REPLACE`**

- Replaces all occurrences of a substring with another substring.
- **Syntax:** `REPLACE(string, search, replace)`
- **Example:**
  ```sql
  SELECT REPLACE('Hello World', 'World', 'SQL'); -- Output: 'Hello SQL'
  ```

---

### **7. `LOCATE` / `POSITION`**

- Finds the position of a substring within a string.
- **Syntax:**
  - `LOCATE(substring, string)` (MySQL)
  - `POSITION(substring IN string)` (ANSI SQL)
- **Example:**
  ```sql
  SELECT LOCATE('World', 'Hello World'); -- Output: 7
  ```

---

### **8. `LEFT` / `RIGHT`**

- Extracts a specified number of characters from the left or right of a string.
- **Syntax:**
  - `LEFT(string, number)`
  - `RIGHT(string, number)`
- **Example:**
  ```sql
  SELECT LEFT('Hello World', 5), RIGHT('Hello World', 5); -- Output: 'Hello', 'World'
  ```

---

### **9. `INSTR`**

- Returns the position of the first occurrence of a substring.
- **Syntax:** `INSTR(string, substring)`
- **Example:**
  ```sql
  SELECT INSTR('Hello World', 'World'); -- Output: 7
  ```

---

### **10. `LPAD` / `RPAD`**

- Pads a string with a specified character on the left (`LPAD`) or right (`RPAD`).
- **Syntax:**
  - `LPAD(string, length, pad_string)`
  - `RPAD(string, length, pad_string)`
- **Example:**
  ```sql
  SELECT LPAD('123', 5, '0'), RPAD('123', 5, '0'); -- Output: '00123', '12300'
  ```

---

### **11. `FORMAT`**

- Formats a number as a string with specific decimal places.
- **Syntax:** `FORMAT(number, decimal_places)`
- **Example:**
  ```sql
  SELECT FORMAT(1234.5678, 2); -- Output: '1,234.57'
  ```

---

### **12. `REVERSE`**

- Reverses a string.
- **Syntax:** `REVERSE(string)`
- **Example:**
  ```sql
  SELECT REVERSE('Hello'); -- Output: 'olleH'
  ```

---

### **13. `ASCII` / `CHAR`**

- `ASCII`: Returns the ASCII value of the first character.
- `CHAR`: Converts an ASCII value to a character.
- **Syntax:**
  - `ASCII(string)`
  - `CHAR(ascii_value)`
- **Example:**
  ```sql
  SELECT ASCII('A'), CHAR(65); -- Output: 65, 'A'
  ```

---

### **14. `LTRIM` / `RTRIM`**

- Removes leading (`LTRIM`) or trailing (`RTRIM`) spaces.
- **Syntax:**
  - `LTRIM(string)`
  - `RTRIM(string)`
- **Example:**
  ```sql
  SELECT LTRIM('   Hello'), RTRIM('Hello   '); -- Output: 'Hello', 'Hello'
  ```

---

### **15. `CHAR_LENGTH` / `LENGTH`**

- `CHAR_LENGTH`: Returns the number of characters in a string.
- `LENGTH`: Returns the number of bytes (useful for multibyte encodings).
- **Syntax:**
  - `CHAR_LENGTH(string)`
  - `LENGTH(string)`
- **Example:**
  ```sql
  SELECT CHAR_LENGTH('Hello'), LENGTH('Hello'); -- Output: 5, 5
  ```
