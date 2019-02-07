# networktacacs-

## 1.installation tacacs+
sudo apt-get install tacacs+

## 2.Edit tac_plus Configuration File
$ vi /etc/tacacs+/tac_plus.conf

## 3.The default configuration of the TACACS+ accounting log is /var/log/tac_plus
accounting file = /var/log/tac_plus.acct

## 4.Define TACACS+ Key
key = tacacskey123

## 5.User Accounts And Groups
```
*************************
***USERS ACCOUNTS HERE***
*************************
user = NetworkEngineer1 {
        member = Network_Engineers
}
user = FieldTech1 {
        member = Field_Techs
}
user = Manager1 {
        member = Managers
}
```

## 6.This section will create groups, specify their authentication method (in this case using /etc/passwd – Linux user authentication), and what authorized commands are available to the groups.

## 7.Network engineers groups 
```
Network engineers have all commands available to them. The default service = permit parameter tells TACACS+ 
#daemon that all commands are allowed for this group. The login = file /etc/passwd parameter tells the daemon 
#to look for the user account and password matches in the file. If it matches, allow the account to log in to 
#the router and switch. The enable = file /etc/passwd tells the daemon that it needs to match the password of 
#the user account. If it matches, allow to enter privileged EXEC mode, also known as enable mode.

*************************
***   GROUPS HERE     ***
*************************
group = Network_Engineers {
        default service = permit
        login = file /etc/passwd
        enable = file /etc/passwd
}
```

## 8.
```
Field Technicians, on the other hand, has a default service = deny parameter which tells the daemon that all other commands 
except for user EXEC commands are allowed. 

group = Field_Techs {
        default service = deny
        login = file /etc/passwd
        enable = file /etc/passwd
        service = exec {
        priv-lvl = 2
        }
        cmd = enable {
                permit .*
        }
        cmd = show {
                permit .*
        }
        cmd = do {
                permit .*
        }
        cmd = exit {
                permit .*
        }
        cmd = configure {
                permit terminal
        }
        cmd = interface {
                permit .*
        }
        cmd = shutdown {
                permit .*
        }
        cmd = no {
                permit shutdown
        }
        cmd = speed {
                permit .*
        }
        cmd = duplex {
                permit .*
        }
        cmd = write {
                permit memory
        }
        cmd = copy {
                permit running-config
        }
}
```

## 9.creating manger group
```
Managers are given access as well, however, the only privilege EXEC mode allowed is show running-config. 
Feel free to add more commands necessary for your boss and/or your boss’ boss.
group = Managers {
        default service = deny
        login = file /etc/passwd
        enable = file /etc/passwd
        service = exec {
        priv-lvl = 2
        }
        cmd = enable {
                permit .*
        }
        cmd = show {
                permit .*
        }
        cmd = exit {
                permit .*
        }
   ```
   
## 10.creating test user+testgroup
```
Creating a test user account and the group might be a good idea, so you can test things out that have been added 
to this configuration. This particular test user will only have a cleartext password.        
user = test {
        member = Test_Group
}
group = Test_Group {
        default service = deny
        service = exec {
        priv-lvl = 2
        login = password1
        enable = password1
}
```
