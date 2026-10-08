# Bài 3: Thiết lập Cơ sở dữ liệu và Tự cấu hình dịch vụ Systemd cho Spring Boot

## 1. Khởi tạo Cơ sở dữ liệu MySQL

Đăng nhập vào MySQL trên VPS (bằng lệnh `sudo mysql -u root -p`) và thực thi các lệnh SQL sau:

```sql
CREATE DATABASE springboot_db;
CREATE USER 'spring-admin'@'localhost' IDENTIFIED BY 'SpringSecure@123';
GRANT ALL PRIVILEGES ON springboot_db.* TO 'spring-admin'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## 2. Tạo tài khoản Linux và chuẩn bị ứng dụng

Mở terminal trên VPS và chạy các lệnh sau:

```bash
# 1. Tạo user hệ thống spring-runner (không có shell login để tăng cường bảo mật)
sudo useradd -r -s /usr/sbin/nologin spring-runner

# 2. Tạo thư mục chứa ứng dụng (trong thực tế, bạn cần copy file app.jar vào đây)
sudo mkdir -p /opt/spring-app/

# 3. Phân quyền thư mục cho user spring-runner
sudo chown -R spring-runner:spring-runner /opt/spring-app/
```
*(Ghi chú: Bài tập giả định rằng tệp `app.jar` đã được tải lên thư mục `/opt/spring-app/` và được cấu hình để lắng nghe cổng 8082)*.

## 3. Cấu hình dịch vụ Systemd

File `spring-app.service` đã được cung cấp đính kèm cùng báo cáo này. 
Để sử dụng, bạn tải file cấu hình đó lên VPS và thực thi các lệnh:

```bash
# Copy tệp dịch vụ vào hệ thống
sudo cp spring-app.service /etc/systemd/system/

# Nạp lại cấu hình systemd
sudo systemctl daemon-reload

# Kích hoạt (chạy cùng hệ thống) và khởi động ứng dụng
sudo systemctl enable spring-app.service
sudo systemctl start spring-app.service
```

## 4. Kiểm tra kết quả

Sau khi dịch vụ khởi chạy, bạn thực hiện kiểm tra và lưu lại kết quả bằng các lệnh dưới đây:

### Trạng thái của dịch vụ
```bash
sudo systemctl status spring-app.service
```
**Kết quả mong đợi:** 
*(Bạn hãy copy đầu ra thực tế hoặc dán ảnh chụp màn hình vào đây. Cần nhìn thấy dòng chữ `Active: active (running)` và tác vụ được chạy dưới user `spring-runner`)*

`![Systemctl_Status](./images/systemctl_status.png)`


### Kiểm tra cổng lắng nghe (Port 8082)
```bash
ss -tlnp | grep 8082
```
**Kết quả mong đợi:** 
*(Dán ảnh chụp màn hình hoặc kết quả vào đây. Cần nhìn thấy cổng 8082 đang LISTEN)*

`![Port_8082](./images/port_8082.png)`
