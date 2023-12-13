# Codify靶機練習(easy)
> Ｏ育 & Ｏ慶

 1.	nmap掃描弱點後發現有三個開啟的port-22(ssh)/ port-80(網站)/port-3000(Node.js Express framework)，可嘗試利用
    ![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/45d5e870-7d9c-454d-9e01-168c59098137)
   	![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/8486c28e-b230-49a1-9397-8270afdb19cf)
 2.	利用port-3000訪問該網站(10.10.11.239:3000)，並尋找可利用的漏洞
    ![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/8256d977-01bf-4563-9ec3-25ed1ba17fa1)
 3.	經過一番尋找後，發現這個頁面使用了Node.js的vm2模型，而詳細研究該模型後，可發現一個漏洞CVE-2023-30547
	  ![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/3c417325-5ba5-41d2-b643-f7d7fddb3a50)
 4.	在 catch 區塊中，通過對 c.constructor('return process')().mainModule.require('child_process').execSync 的呼叫，執行了一個惡意的 shell 指令，這個指令的目的是建立一個反向 shell 連線到 10.10.14.118 的 IP 位址上的 1234 端口。
  	![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/b1623119-57f5-45f0-88db-42c19b107a3f)
 5.	建立監聽，以接收反向 shell 連線
    ![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/23b6267a-53dc-45c5-a17b-9fb85972f4a9)
 6.	搜尋各檔案後，發現tickets.db，進而發現使用者名稱與密碼hash值
    ![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/e2a21244-5231-419b-9ba0-328d96ce5308)
 7.	利用John the Ripper破解密碼hash值，取得密碼
    ![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/c90109dc-e124-4e5f-8834-07de9c95c0df)
 8.	利用port-22(ssh)使用剛取得使用者與密碼進行連線
    ![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/c48b1bb8-786b-4f4c-9703-4912a22df848)
 9.	第一階段完成，取得user的金鑰
    ![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/dc1ddc3d-b2b8-454a-af30-3e6165ac0940)
---
 1.	先檢查自己擁有的權限
    ![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/096121e5-f5cc-4f8e-a03c-44309a32b23f)
 2.	腳本的這一部分將使用者提供的密碼（USER_PASS）與實際的資料庫密碼（DB_PASS）進行比較。 此處的漏洞是由於在Bash中使用[[ ]]中的==所致，它執行模式匹配而不是直接字符串比較，這意味著使用者輸入（USER_PASS）被視為一種模式，如果它包含*或？ 等通配字元，則它可能會匹配意外的字串
  	![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/8b2e94ec-e5e7-4690-9066-7f44798e844a)
 3.	這是一個簡單的 Python 腳本，透過一個逐位元攻擊（brute-force）的方式來破解 MySQL 數據庫的密碼。
    ![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/37619988-fbae-448b-ace9-66ca1f3a8b52)
 4.	使用 Vim 建立test.py並賦予其權限執行該腳本
    ![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/91056eef-3c27-47a6-91ed-75659b3b952d)
 5.	第二階段完成，取得root的金鑰
    
    ![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/124786200/0bb8673c-9afa-42f1-8df9-73783cfddea5)







   	




