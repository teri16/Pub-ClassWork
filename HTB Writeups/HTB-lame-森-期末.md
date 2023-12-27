# HTB lame (尚未完成)
## 偵查

```
nmap -A -Pn [ip address]
```
21 22 139 445 port 有開啟
![image](https://hackmd.io/_uploads/HyAXPHuDp.png)

---
### 21port 
![image](https://hackmd.io/_uploads/SyA4-dKDa.png)

use `searchsploit`
![image](https://hackmd.io/_uploads/rkusAvFwa.png)
使用 metasploit framework
搜尋vsft 使用1
![image](https://hackmd.io/_uploads/BkcU1tKvT.png)
設定RHost，exploit，失敗
![image](https://hackmd.io/_uploads/H1kjyYFDT.png)


### 22port 
ssh通常是無法成功攻擊
![image](https://hackmd.io/_uploads/HkIE0dYPT.png)

### 139 445port
同樣都是samba服務
![image](https://hackmd.io/_uploads/BJ8-Httwa.png)
查詢smaba版本(有metasploit)
![image](https://hackmd.io/_uploads/Bk90w2tPa.png)
使用metasploit framework 
![image](https://hackmd.io/_uploads/Hy07_3KPa.png)
查看option
![image](https://hackmd.io/_uploads/SyiUu2twa.png)

準備使用4444port監聽
![image](https://hackmd.io/_uploads/HkKownFwp.png)

---
設定參數
![image](https://hackmd.io/_uploads/r1fNKntwa.png)

仍然失敗
上網搜尋漏洞github
![image](https://hackmd.io/_uploads/S1Ml33tDa.png)

需要建立shell code
![image](https://hackmd.io/_uploads/ByY6s2YP6.png)

smbmap

smbclient
