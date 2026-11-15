# Locked Notes

## Investigation

I started by listing the contents of the provided backup:

```bash
tar -tzvf securenotes_data.tar.gz
```

It contained an SQLite database, shared preferences and logcat files.

I initially noticed a `last_backup_token` value in the shared preferences, but decoding it turned out to be a dead end.

I then checked the logcat and found an entry showing that note `_id=4` had been opened while locked.

Next, I opened the SQLite database:

```bash
sqlite3 extracted/com.larkfield.securenotes/databases/notes.db
```

I queried the notes table:

```sql
SELECT _id, title, body, is_locked FROM notes;
```

Note ID `4`, titled `Recovery Code`, caught my attention. Its body was stored as a BLOB, and the first bytes were `1f8b`, which indicated GZIP data.

I extracted that BLOB using Python:

```python
import sqlite3

db = sqlite3.connect("extracted/com.larkfield.securenotes/databases/notes.db")
body = db.execute("SELECT body FROM notes WHERE _id = 4").fetchone()[0]

with open("note.gz", "wb") as f:
    f.write(body)
```

I checked and extracted the resulting file:

```bash
gzip -t note.gz
zcat note.gz
```

This revealed the flag.

## Flag

```text
InIt{s3cur3_n0t3s_n0t_s0_s3cur3}
```