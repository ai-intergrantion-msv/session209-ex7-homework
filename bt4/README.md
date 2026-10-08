# Bài 4: Cấu hình Reverse Proxy Nginx cho ứng dụng Spring Boot

## 1. Chuẩn bị nội dung trang Web tĩnh

Thực thi các lệnh sau trên VPS để tạo trang tĩnh (index.html) với thông tin học viên:

```bash
# Đảm bảo thư mục tồn tại
sudo mkdir -p /var/www/html

# Tạo tệp index.html
sudo bash -c 'cat > /var/www/html/index.html <<EOF
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Trang Chu Hoc Vien</title>
</head>
<body>
    <h1>Ho va ten: [Điền tên của bạn vào đây]</h1>
    <h2>Ma lop: [Điền mã lớp vào đây]</h2>
</body>
</html>
EOF'
```

## 2. Thiết lập Nginx Reverse Proxy

File `spring-proxy.conf` đã được cung cấp kèm báo cáo này. 
Bạn hãy đưa file cấu hình này lên máy chủ và thực hiện tuần tự các lệnh sau:

```bash
# 1. Xóa cấu hình mặc định (default) của Nginx nếu có để tránh xung đột cổng 80
sudo rm -f /etc/nginx/sites-enabled/default

# 2. Copy file cấu hình mới vào sites-available
sudo cp spring-proxy.conf /etc/nginx/sites-available/

# 3. Kích hoạt bằng cách tạo liên kết mềm (symlink) sang sites-enabled
sudo ln -s /etc/nginx/sites-available/spring-proxy.conf /etc/nginx/sites-enabled/

# 4. Kiểm tra cú pháp Nginx
sudo nginx -t

# 5. Khởi động lại dịch vụ Nginx nếu kiểm tra cú pháp thành công
sudo systemctl reload nginx
```

## 3. Kiểm tra kết quả

Sau khi cài đặt thành công, bạn hãy chạy các lệnh kiểm tra và điền kết quả vào bên dưới:

### Kiểm tra cú pháp Nginx (`sudo nginx -t`)
**Kết quả của bạn:**
*(Hãy copy đoạn text kết quả báo `syntax is ok` và `test is successful` thay thế vào đây)*

`![Kiem_tra_Nginx](./images/nginx_t.png)`


### Kiểm tra trang tĩnh (`curl -I http://localhost/`)
Lệnh này nhằm đảm bảo trang web tĩnh đang hoạt động bình thường, mã trả về là 200.
**Kết quả của bạn:**
*(Copy đoạn text xuất ra màn hình hoặc dán ảnh vào đây)*

`![Test_Trang_Chu](./images/curl_root.png)`


### Kiểm tra Reverse Proxy (`curl -I http://localhost/api/health`)
*(Lưu ý: Giả sử ứng dụng Spring Boot cổng 8082 đang chạy bình thường ở bài 3 và có endpoint `/health`)*
Lệnh này xác nhận định tuyến API hoạt động, không bị trả về 502 Bad Gateway.
**Kết quả của bạn:**
*(Copy đoạn text xuất ra màn hình hoặc dán ảnh vào đây)*

`![Test_API](./images/curl_api.png)`
