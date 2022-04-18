# Superset, công cụ mạnh và miễn phí cho người làm dữ liệu

Chúng ta đang bước vào kỷ nguyên số với nhu cầu lưu trữ và khai thác các nguồn dữ liệu ngày một lớn. Trong các công cụ phân tích dữ liệu, Power BI được đánh giá cao do tương đối dễ sử dụng, việc triển khai cũng uyển chuyển do có thể triển khai trên nhiều nền tảng như máy tính, thiết bị di động... 
Tuy nhiên, việc triển khai cho nhu cầu phụ vụ báo cáo cho số đông thì với Power BI cũng có một trở ngại, đó là chi phí hàng tháng cho bản máy chủ lên tới 5000$.
Để giải quyết vấn đề chi phí, Apache Superset có thể là một lựa chọn tốt để phục vụ nhu cầu triển khai báo cáo ở quy mô lớn. Ứng dụng bắt đầu như một dự án hack-a-thon của Maxime Beauchemin (người tạo ra Apache Airflow) khi làm việc tại Airbnb và tham gia chương trình Apache Incubator vào năm 2017. 
Ngoài Airbnb, dự án còn có sự đóng góp đáng kể từ các công ty công nghệ hàng đầu khác, bao gồm Lyft và Dropbox. Superset tốt nghiệp chương trình vườn ươm và trở thành dự án cấp cao nhất tại Apache Software Foundation vào năm 2021.
Về cơ bản, các tính năng của Superset khá là tương đồng các phần mềm phân tích dữ liệu khác.
- Tạo và quản lý dashboard
- Xác thực với các hệ thống xác thực của doanh nghiệp (OpenID, LDAP, OAuth...)
- Hỗ trợ nhiều loại cơ sở dữ liệu: SQL, Postgres, MySQL...
- Hỗ trợ truy vấn trực tiếp

# Bắt đầu.

Để dễ thử các tính năng, ta có thể dùng docker hoặc docker compose.

docker run -d -p 8080:8088 --name superset apache/superset

Đợi 1 tí để docker kéo về là có thể bắt đầu sử dụng
Sau khi docker ở trạng thái chạy. Ta sẽ cấu hình superset.

Đầu tiên là admin account

``` 
docker exec -it superset superset fab create-admin --username admin --firstname Superset --lastname Admin --email admin@superset.com --password admin
```

Nâng cấp CSDL tí

```
docker exec -it superset superset db upgrade
```

Tải vài ví dụ

```
docker exec -it superset superset load_examples
```

Chạy một lần cuối để khởi động

```
docker exec -it superset superset init
```

Cơm thêm: bản docker gốc đã hỗ trợ nhiều kết nối đến các loại CSDL thông dụng, tuy nhiên, bạn hoàn toàn có thể cài đặt thêm 1 cách dễ dàng bằng cách chạy lệnh trong docker. Ví dụ:muốn cài driver để kết nối đến SQL server.

```
docker exec -it superset pip install pymssql
```

Các driver mà Superset hỗ trợ tương đối nhiều. Bạn có thể xem trong danh sách bên dưới để cài thêm

https://superset.apache.org/docs/databases/installing-database-drivers

Bây giờ ta có thể bắt đầu

```
http://localhost:8080/login/ 
```

Đăng nhập bằng tài khoản đã khai báo ở trên: [admin/admin]

Màn hình welcome của Superset liệt kê các bảng biểu, đồ thị, báo cáo đã được thao tác trước đó trong hệ thống để dễ dàng truy cập.

[image](superset 1.png)

Bước 1: Kết nối dữ liệu
Apache Superset không lưu trữ dữ liệu nên cần phải kết nối đến một CSDL. Superset cung cấp một công cụ trực quan để thao tác việc kết nối. Nhìn chung thao tác kết nối khá đơn giản. Tuỳ vào loại dữ liệu nguồn mà ta cung cấp các thông tin kết nối tương ứng như username, password.

[image](superset 2.1.png)

Ta cũng có thể kết nối đến CSDL bằng cách cung cấp URI theo cấu trúc SQLAlchemy

[image](superset 2.2.png)

Sau khi kết nối, ta có thể bắt đầu tiến hành tạo các tập data (dataset) để vẽ biểu đồ.
Superset cung cấp giao diện để nhìn tổng quan các dataset có thể thao tác.

[image](superset 3.png)

Ta có thể tiếp tục làm việc với các dataset đang có, hoặc tạo 1 dataset mới bằng cách kết nối đến CSDL, chọn schema và bảng.

[image](superset 4.png)

Sau khi tạo dataset, ta có thể điều chỉnh cấu hình của dataset như thêm các metric, điều chỉnh cấu hình của dữ liệu như: cột được phép lọc, cột dữ liệu, hoặc thêm các cột tính toán.

[image](superset 5.1.png)
[image](superset 5.2.png)
[image](superset 5.3.png)

Sau khi có tập dữ liệu, ta có thể tiến hành tạo biểu đồ.
Superset cung cấp rất nhiều loại biểu đồ, công việc cần làm là chọn dataset và loại biểu đồ muốn hiển thị

[image](superset 6.png)

Sau đó, ta có thể điều chỉnh lại tuỳ theo nhu cầu cần hiển thị, Superset cung cấp rất nhiều tuỳ chỉnh tuỳ theo biểu đồ và dữ liệu.

[image](superset 7.png)

Bước cuối cùng là đưa tất cả các biểu đồ lên dashboard.

[image](superset 8.png)

Ngoài luồng giao diện phục vụ cho việc tạo bảng biểu, Superset còn cung cấp công cụ cho phép người dùng truy vấn dữ liệu, lưu lại các truy vấn và xem lịch sử truy vấn của người dùng.

[image](superset 8.png)

Kết
Apache Superset còn khá mới, tuy nhiên phần giao diện khá hoàn chỉnh và cung cấp công cụ trực quan cho người dùng cần làm phân tích dữ liệu, báo cáo, bảng biểu. Superset cũng cho phép trích bảng biểu để nhúng vào các màn hình ứng dụng, gởi email trực tiếp chứa đường dẫn báo cáo, xuất hình ảnh báo cáo để chia sẻ... Nhìn chung Superset đáp ứng được phần lớn nhu cầu của người làm phân tích dữ liệu và báo cáo.

Vậy Superset có thể thay thế được các công cụ khác không? 
Câu trả lời là cũng không hẳn. Superset còn tồn tại nhiều điểm cần phải khác phục:
- Việc kết nối đến các CSDL đôi khi rất khó khăn do thiếu tài liệu. Vd để kết nối đến databricks thì không có hướng dẫn đầy đủ và phải tự điều chỉnh trên thông tin kết nối được cung cấp bởi databricks.
- Luồng giao diện hơi lạ và không được thân thiện nếu so sánh với PowerBI

Vậy Superset phù hợp với ai:
- Những người cần công cụ mạnh để có thể làm nhiều loại bảng, query liên tục trên CSDL thì không cần phải dùng thêm công cụ khác.
- Những người đang tìm kiếm một công cụ làm phân tích dữ liệu miễn phí và mạnh mẽ không kém các công cụ nổi tiếng.
- Khả năng tuỳ chỉnh cực kỳ lớn, hỗ trợ nhiều loại dữ liệu nguồn, và có thể cài đặt thêm nhiều loại biểu đồ chưa có.