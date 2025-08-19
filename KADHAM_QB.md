
# SQL QUESTION BANK
---

## 1. How can I retrieve the name of the current Oracle database I'm connected to?

### 🧾 SQL Query
```sql
SELECT sys_context('USERENV', 'DB_NAME') AS database_name FROM dual;
```

### 📊 Sample Output

| DATABASE_NAME |
|---------------|
| AEXTPDB       |

---

Here’s the next entry in the same format, focused on retrieving the **current user name** in Oracle:

---

## 2. How can I retrieve the name of the current Oracle user?

### 🧾 SQL Query
```sql
SELECT sys_context('USERENV', 'CURRENT_USER') AS current_user FROM dual;
```

### 📊 Sample Output

| CURRENT_USER |
|--------------|
| WKSP_CTSDB |

---
