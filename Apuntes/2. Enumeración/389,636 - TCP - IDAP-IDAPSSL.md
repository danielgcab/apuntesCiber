## Ldapsearch

```
ldapsearch -x -H ldap://<IP> -s base namingcontexts # enumeración básica
ldapsearch -x -H ldap://<IP> -D "cn=username,dc=domain,dc=htb" -b "dc=domain,dc=htb" -w 
```