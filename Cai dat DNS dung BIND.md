##1. Cài đặt
Sau khi thiết lập địa chỉ IP tĩnh cho máy chủ đảm nhận vai trò DNS Server, bạn thực hiện các lệnh sau để cài đặt gói BIND9 (dùng cấu hình dịch vụ DNS trên Linux):

`# sudo -i`
<br>`# apt-get update`
<br>`# apt-get install bind9`
<br>Tiếp theo chọn **y**

Sau khi cài đặt thành công, thu được thu mục chính là /etc/bind. Trong thư mục này, sẽ cấu hình các file sau:

`/etc/bind/named.conf.local`
<br>`/etc/bind/db.local`

File cơ sở dữ liệu DNS (chẳng hạn /etc/bind/db.hoang2811.com)

##2. Cấu hình

Trước tiên, mở file /etc/bind/named.conf.local và bổ sung một zone mới, chẳng hạn hoang2811.com:

                        zone "hoang2811.com" {
                              type master;
                              file "/etc/bind/db.hoang2811.com";
                        };

Tiếp theo, bạn tạo file cơ sở dữ liệu DNS /etc/bind/db.hoang2811.com bằng cách sao chép nội dung từ file mẫu /etc/bind/db.local:

`# cp /etc/bind/db.local /etc/bind/db.hoang2811.com`

Đến đây, bạn hiệu chỉnh file /etc/bind/db.hoang2811.com bằng cách thay localhost. bằng tên đầy đủ của máy chủ DNS (FQDN). Thay 127.0.0.1 bằng địa chỉ IP của DNS Server và root.localhost thành một địa chỉ email chính xác, nhưng sử dụng dấu chấm "." thay vì biểu tượng "@”. Đồng thời, cũng tạo ra một bản ghi (A) tương ứng với ns.hoang2811.com. Nội dung của file sẽ như sau:

              ;
              ; BIND data file for local loopback interface
              ;
              $TTL 604800
              @    IN    SOA   ns.hoanghac.org.  root.hoang2811.com. (
                                                               2          ; Serial
                                                          604800          ; Refresh
                                                           86400          ; Retry
                                                         2419200          ; Expire
                                                          604800 )        ; Negative Cache TTL
              ;
              @    IN    NS ns.hoang2811.com.
              @    IN    A 192.168.1.10
              @    IN    AAAA ::1
              ns   IN    A 192.168.1.10

Tiếp theo, bạn khởi động lại dịch vụ BIND9 để những thao tác cấu hình vừa thực hiện có hiệu lực:

`# /etc/init.d/bind9 restart`

##3. Kiểm tra

Để kiểm tra hoạt động của DNS Server, bạn sử dụng lệnh ping như sau:

`# ping ns.hoang2811.com`

Hoặc có thể dùng lệnh **dig** or **nslookup**
