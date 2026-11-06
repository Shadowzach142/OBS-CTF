admin'--
**Why it works:**

- `--` is a single-line comment in SQL. Injecting `admin' --` closes the username literal and comments out the remainder of the WHERE clause (including the password check).
 
 
 admin' /*
 **Why it works:**

- SQLite supports C-style comments `/* ... */`. Injecting `/*` after closing the username literal opens a comment block that consumes the remainder of the SQL statement, effectively removing the password constraint.

admin';
	**Why it works:**

- The semicolon `;` terminates the current SQL statement. If the application executes the injected text as part of a multi-statement execution, the `;` ends the `SELECT` that checks the username before the password check is parsed. The password check then becomes a separate (often ignored) fragment.

ad'||'min'; 
	In SQLite you can concatenate string literals with the `||` operator, e.g. `ad'||'min` → `admin`. If `admin` is filtered but `ad` and `min` are allowed, you can reconstruct the username at runtime
	
adm'||'in';
	concatenation operator `||` to rebuild `admin` from fragments.

adm'||'in
' glob '*

The `glob` keyword is used to match text values against a pattern using wildcards. Since we provided the `*` wildcard, we are saying: Give us a user with the username `admin` and any password.

adm'||'in
' glob '*