# Bài 2: Quản trị Tường lửa UFW cho Cụm Dịch vụ Multi-port

## 1. Các lệnh cấu hình UFW đã thực thi

Theo yêu cầu, hệ thống sẽ được cấu hình tường lửa chặn mặc định kết nối từ bên ngoài vào, mở các cổng 22 (SSH), 80 (Web), 8082 (Spring Boot), và không mở cổng 3306 (MySQL) để bảo vệ database.

Dưới đây là các lệnh cần thực thi trên máy chủ (VPS):

```bash
# 1. Đặt chính sách mặc định: chặn chiều vào, cho phép chiều ra
sudo ufw default deny incoming
sudo ufw default allow outgoing

# 2. Cho phép các cổng dịch vụ theo yêu cầu
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 8082/tcp

# 3. Kích hoạt tường lửa UFW (nhấn 'y' rồi Enter nếu được hỏi)
sudo ufw enable
```

## 2. Kết quả kiểm tra

Sau khi thiết lập xong, chạy lệnh dưới đây để xem chi tiết trạng thái của UFW:

```bash
sudo ufw status verbose
```

**Đầu ra của lệnh `sudo ufw status verbose`:**

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere                  
80/tcp                     ALLOW IN    Anywhere                  
8082/tcp                   ALLOW IN    Anywhere                  
22/tcp (v6)                ALLOW IN    Anywhere (v6)             
80/tcp (v6)                ALLOW IN    Anywhere (v6)             
8082/tcp (v6)              ALLOW IN    Anywhere (v6)             
```

---
*Lưu ý cho học viên:*
*Bạn hãy thực thi các lệnh trên VPS của mình, sau đó sao chép (copy) toàn bộ đầu ra thực tế của lệnh kiểm tra `sudo ufw status verbose` và ghi đè vào khối văn bản mẫu ở mục 2 để hoàn thiện bài nộp trên GitHub.*
