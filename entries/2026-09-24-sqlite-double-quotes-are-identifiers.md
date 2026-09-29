# SQLite: double quotes are identifiers, not strings

`SELECT * FROM runs WHERE outcome="lyrics"` returns zero rows when the table also has a column named `lyrics`, because SQLite reads `"lyrics"` as that column and compares `outcome` to it row by row. Double-quoted text only falls back to a string literal when no such identifier exists, and that fallback is a documented legacy misfeature.

Use single quotes or a bound parameter:

```sql
SELECT * FROM runs WHERE outcome = 'lyrics';
```

Verified 2026-09-24.
