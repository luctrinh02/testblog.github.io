---
layout: post
title:  "Cách cài đặt và sử dụng Flyway cho window"
date:   2021-11-14 09:50:24 +0700
categories: jekyll update
---
## Giới Thiệu:
Flyway là một công cụ di chuyển cơ sở dữ liệu mã nguồn mở. Nó được ủng hộ mạnh mẽ vì quy ước và cấu hình đơn giản  
Nó chỉ dựa trên 7 lệnh cơ bản: Migrate, Clean, Info, Validate, Undo, Baseline and Repair.  
Các phép di chuyển có thể được viết bằng SQL (cú pháp dành riêng cho cơ sở dữ liệu (chẳng hạn như PL / SQL, T-SQL, ...) được hỗ trợ) hoặc Java (để chuyển đổi dữ liệu nâng cao hoặc xử lý các LOB).  
Hướng dẫn ngắn gọn này sẽ giúp bạn cách thiết lập và chạy với công cụ Flyway Command-line và sử dụng trên cơ sở dữ liệu MSSQL.  
Bạn sẽ mất khoảng 10 phút để hoàn thành hướng dẫn này.   

## Tải và cài đặt Flyway Command-line và spawnctl.exe 
### I.Tải Flyway Command-line và spawnctl.exe
[Tải Flyway Command-line](https://flywaydb.org/download)  
[Tải spawnctl.exe](https://drive.google.com/file/d/1edfyclnAnEghc52cEOuM8tLvnSYZqgKQ/view?usp=sharing)  
pawnctl.exe không phải là một trình cài đặt, nó là một tệp thực thi dòng lệnh có thể chạy trực tiếp. Bạn nên tạo thư mục **C: \ Program Files \ Spawn** và lưu nó ở đó để dễ dàng thao tác.
Đồng thời bạn cũng nên lưu và giải nén Flyway Command-line tại thư mục này.
### II.Sử dụng spawnctl.exe với CMD
1.Lưu patch cài đặt sqwnctl.exe để có thể gọi bằng cmd.  
Các bạn mở **Control Panel** --> **System and Security** -->System  
Tại tab **Related settings** --> **Advanced system settings**  
![](https://user-images.githubusercontent.com/94233704/141674523-bff7417d-3ab2-42f5-b7fc-927519abb09b.png)  
  
Tại tab **Avanced** chọn **Environment Variables**  
![](https://user-images.githubusercontent.com/94233704/141674685-579a165c-f8fd-4345-bd64-81d76fb59941.PNG)  
  
Tại tab **System Variable** tìm đến variable **Path**  
![](https://user-images.githubusercontent.com/94233704/141674687-0d72e0bf-7841-4f97-bcec-e62588734fa3.PNG)  
  
Chọn **Edit** --> **New** --> nhập đường dẫn bạn lưu file spqwnctl.exe vào.  
Ở đây tôi để là **C: \ Program Files \ Spawn**  
  
![](https://user-images.githubusercontent.com/94233704/141674688-260576e0-1ab3-4021-b39c-cdfe406451af.PNG)  
  
Chọn **Ok** ở tất các các cửa sổ đang mở khi thay đổi đường dẫn  

2.Mở **CMD** bằng quyền quản trị  
Bạn mở **CMD** và chạy với quyền quản trị bằng cách gõ 'CMD' vào thanh tìm kiếm. Sau đó nhấn chuột phải vào 'Command Promt' chọn 'Run as Admintrator'.
Hoặc bạn có thể nhấn tổ hợp phím **Window + R** gõ 'cmd' sau đó nhấn tổ hợp phím **Ctrl + Shift + Enter**.  
3.Gọi spawnctl.exe bằng cmd  
Tại cmd bạn gõ lệnh <code>spawnctl</code> . Một bảng hiện thị các lệnh bạn có thể dử dụng với spawnctl hiện lên
![](https://user-images.githubusercontent.com/94233704/141676084-f8ed56b4-3fc3-4a81-b133-80b10213b34c.PNG)  
  
4.Chỉnh sửa file **flyway.conf**  
Các bạn giải nén file **Flyway Command-line** đã cài đặt vào **C: \ Program Files \ Spawn**  
Các bạn tìm đến file **flyway.conf** theo đường dẫn  
**C:\Program Files\Spawn\flyway-8.0.4\conf\flyway.conf**  
Tại file **flyway.conf** các bạn sửa thành  
<code>
    flyway.url=[url]<br>
    flyway.user=[User]<br>
    flyway.password=[Password]<br>
</code>
Ví dụ tôi sẽ sửa thành  
<code>
    flyway.url=jdbc:sqlserver://DESKTOP-37RFLV9\\SQLEXPRESS:1433;databaseName=EduSys<br>
    flyway.user=sa<br>
    flyway.password=123<br>
</code>
Sau đó các bạn lưu và đóng file **flyway.conf** lại  
5.Thực hiện migrate  
Đầu tiên các bạn mở cmd và trỏ tới địa chỉ của file **flyway-8.0.4** bằng lệnh  
<code>cd "C:\Program Files\Spawn\flyway-8.0.4"</code>  
![](https://user-images.githubusercontent.com/94233704/141676808-410efaf5-eb1c-475e-882f-657f4282d256.PNG)  
  
Sau đó cmd đã được trỏ về file **flyway-8.0.4**  
Để có thể thực hiện được việc migrate chúng ta cần lưu các phiên bản .sql vào thư mục **C:\Program Files\Spawn\flyway-8.0.4\sql**  
Ví dụ ở đây tôi có file **V1__Create_person_table.sql**  
<code>
    create table PERSON (
    ID int not null,
    NAME varchar(100) not null
);
</code>
Để có thể migrate được file sql này. Ta gõ lệnh <code>flyway -baselineOnMigrate="true" migrate</code>
![](https://user-images.githubusercontent.com/94233704/141677904-4e88e27f-946f-4637-aa27-5da0424ebd29.PNG)  
  
Bạn có thể xem thông tin phiên bản của database bằng cách dùng lệnh <code>flyway info</code>  
![](https://user-images.githubusercontent.com/94233704/141677906-22148a07-d540-414c-9aee-1c94e55bee3a.PNG)  
  
Tiếp theo chúng ta hãy cũng thử thêm một vài dữ liệu thông qua phiên bản **V2__ADD_person_table.sql**  
<code>
insert into PERSON (ID, NAME) values (1, 'Axel');<br>
insert into PERSON (ID, NAME) values (2, 'Mr. Foo');<br>
insert into PERSON (ID, NAME) values (3, 'Ms. Bar');<br>
</code>
Sử dụng cmd ta gõ lệnh <code>flyway -baselineOnMigrate="true" migrate</code>
![](https://user-images.githubusercontent.com/94233704/141677907-5e7a0f7b-8d23-41fd-b0d3-e8a3112da1e5.PNG)  
  
Tiếp tục dùng lệnh <code>flyway info</code>  
![](https://user-images.githubusercontent.com/94233704/141677909-83e61c41-3e44-4f73-80c4-c69f2b9e43d0.PNG)  
  
Và bây giờ database của chúng ta đang ở tại v2

6.Thực hiện undo  
Để có thể trở về v1 chúng ta cần sửa dụng đến undo.  
Để có thể sử dụng undo chúng ta cần một file sql chưa các câu lệnh thực hiện undo  
Ví dụ để thực hiện undo về v1 ta có file **U2__CreateTable_person.sql**
<code>delete from PERSON</code>
Để thực hiện undo chúng ta dùng lệnh <code>flyway undo</code> trong cmd  
![](https://user-images.githubusercontent.com/94233704/141678218-467312bd-104a-40de-8624-edada7fa7123.PNG)  
  
Dùng lệnh <code>flyway info</code> để kiểm tra  
![](https://user-images.githubusercontent.com/94233704/141678220-f8ebd763-b854-40ba-ac8b-f445163206db.PNG)  
  
Bây giờ chúng ta tháy rằng việc undo đã được thực hiện và v2 đang ở trạng thái 'pending' tức là có thể thực hiên.  

### III.Mở rộng
1.Undo về vị trí được chọn  
![](https://user-images.githubusercontent.com/94233704/141678683-fa6da192-027f-43ca-9cd8-fc4b12257f1b.PNG)  
  
Tại đây phiên bản của database đang là v3. Nếu muốn trở về v1 chúng ta phải sử dụng đến <code>-cherryPick</code>  
Cú pháp để trở về v1 sẽ là <code>flyway -cherryPick=2 undo</code>  
![](https://user-images.githubusercontent.com/94233704/141678677-d0f0d46a-11b1-4911-abe0-0e88c6604f8d.PNG)  
  
Khi đấy trạng thái của v3 sẽ là Undone và của v2 sẽ là ignore. Tức là hiện phiên bản duy nhất được thực hiện chính là v1  
![](https://user-images.githubusercontent.com/94233704/141678678-036ae2af-aef3-444d-ab88-37251e4e5c33.PNG)  
  
Do đó ta có thể dễ dàng trở về phiên bản v1.  

2.Quy tắc đặt tên file sql
![](https://user-images.githubusercontent.com/94233704/141678783-5ec46ede-5244-4d32-9fda-a23073735ddc.png)  
  
-prefix: Có thể sử dụng prefix khác, default: V cho versioned migrations, U cho Undo migrations.  
-version: (chỉ áp dung cho Versioned migrations) có thể thêm dấu . hoặc _ để thêm minor version, ví dụ như : V1_2  
-separator: 2 dấu gạch dưới __  
-description: Dấu gạch dưới hoặc khoảng trắng để phân biệt các từ.  
Ví dụ: **V2__ADD_person_table.sql** , **V2.1__ADD_person_table.sql** ,**U2__CreateTable_person.sql**  
