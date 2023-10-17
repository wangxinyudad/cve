SQL injection vulnerability exists in Tongda OA

version:v2017 version and versions below v11.10

Route: general/system/res_manage/monitor/delete_webmail.php

There is an injected parameter: $DELETE_STR

The code here is very concise. When $DELETE_STR is not empty, the parameters are directly spliced ​​into the SQL statement. Since the parentheses are closed here, there is a bypass.

![image](https://github.com/wangxinyudad/cve/assets/62500690/76ad10dc-b300-4345-829d-867ce89947fa)

We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.

POC
```
We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.
```

![image](https://github.com/wangxinyudad/cve/assets/62500690/0abf649f-e4c0-41d9-920f-47b0e79c5955)

And when we change 116 to 115, there will be no delay, indicating the existence of SQL injection.

POC
```
1)%20and%20(substr(DATABASE(),1,1))=char(115)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```

![image](https://github.com/wangxinyudad/cve/assets/62500690/a0af8268-7427-4729-a1e1-5937d025cf96)
