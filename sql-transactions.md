Here is a simple example of how to use `START TRANSACTION`, `COMMIT`, and `ROLLBACK` with a single `UPDATE` statement in MySQL.

### 📝 Transaction Example

This example assumes you have a table named **`products`** with columns like **`product_id`** and **`stock_quantity`**.

---

## 🚀 The Scenario

We want to reduce the stock quantity for a product. We will first start a transaction, perform the update, and then decide whether to make the change permanent (`COMMIT`) or discard it (`ROLLBACK`).

### 1\. Start the Transaction

This tells the database to begin recording a sequence of SQL statements. All changes made after this point are **temporary** until you explicitly commit or rollback.

```sql
START TRANSACTION;
```

---

### 2\. Perform the Update

This is the SQL statement that modifies the data. In a transaction, this change is only visible to the current session until committed.

```sql
UPDATE products
SET stock_quantity = stock_quantity - 5
WHERE product_id = 101;
```

---

### 3\. Commit (Save) or Rollback (Undo)

You have two choices here:

#### A. ✅ Commit the Change (Make it Permanent)

If everything looks good, use `COMMIT` to finalize the changes and make them visible to all other users/sessions.

```sql
-- Use this if the update was successful and you want to save it
COMMIT;
```

#### B. ❌ Rollback the Change (Undo the Update)

If something goes wrong (e.g., the update failed, or you realize you made a mistake), use `ROLLBACK` to discard the changes, and the data will revert to its state before `START TRANSACTION`.

```sql
-- Use this if you need to discard the update and revert the data
ROLLBACK;
```

---

## 💡 Summary

| Command             | Purpose                                                 |
| :------------------ | :------------------------------------------------------ |
| `START TRANSACTION` | Begins a logical unit of work.                          |
| `UPDATE`            | Modifies the data temporarily within the transaction.   |
| `COMMIT`            | Makes all temporary changes permanent.                  |
| `ROLLBACK`          | Undoes all temporary changes since `START TRANSACTION`. |

Would you like to see an example of how to check the original and updated data within the transaction before committing?
