#Machine-Sau(Easy)
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/fcab7274-02ec-4f42-8e90-73f420f4d6c9)
1.利用nmap 掃描發現port 22、80和55555有開，其中port 22的服務是ssh但沒有相關資訊，故選擇http的服務，又port 80顯示被filter，所以最後選擇port 55555進行exploit
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/9efb600c-c345-470a-a60e-735d2e8a20fc)
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/5d463aca-6d55-472f-9f19-c3b5bec8c51a)
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/23df4052-af47-430c-a701-977b30e9015f)
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/691df843-b205-43b1-ad7f-4ceb643fa096)
2.在瀏覽器上開啟55555網頁，發現request-baskets的版本是1.2.1，可藉此進入port80
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/bee206bc-8dfc-4a06-9fe9-4a79981b1ba3)
3.創建一個basket，forward URL
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/29c34897-4274-4fe2-841a-e34b32b347e1)
4.查看basket，發現網頁的版本為Maltrail (v0.53)
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/061eeded-045a-4e36-9322-be6a204fb3d4)
5.到github 複製exploit.py程式碼到kali
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/8526a8f0-a5de-475d-94e8-f83ba3cabae4)
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/7dddc28e-3aa1-4703-aece-eb9ec000f3a1)
6.開port4444監聽，並執行exploit.py
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/b94b135c-3b09-4cbf-8cbb-7f0051695934)
7.查看各個檔案，發現user.txt，取得第一階flag
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/5362da32-aa81-41d2-acf0-ee90f64503d9)
8.取得root權限
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/fdcaa546-a178-4cfe-9ca0-c0f57bb66cdb)
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/11324959-be93-4fb7-a28e-bdd417a33525)
9.取得第二階flag
![image](https://github.com/TwMoonBear-Arsenal/Pub-ClassWork/assets/144238471/e2f89aac-7445-4690-8977-5c51e1819a16)


