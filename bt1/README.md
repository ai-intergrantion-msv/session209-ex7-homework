# Bài 1: Quản lý người dùng giới hạn và Truyền tải dữ liệu qua SFTP trên Windows

## 1. Khởi tạo tài khoản và chuẩn bị dữ liệu trên VPS

Để tạo tài khoản `sftp-user` và chuẩn bị dữ liệu theo yêu cầu, bạn hãy đăng nhập vào máy chủ (VPS) Linux của mình và thực thi các lệnh sau:

```bash
# 1. Tạo tài khoản sftp-user (nhập mật khẩu an toàn khi được hỏi)
sudo adduser sftp-user

# 2. Tạo thư mục log giả lập
sudo mkdir -p /var/log/app-backup/

# 3. Tạo tệp log giả lập
sudo touch /var/log/app-backup/backup-check.log
sudo bash -c 'echo "Backup status: SUCCESS at $(date)" > /var/log/app-backup/backup-check.log'

# 4. Gán quyền cho user sftp-user
sudo chown -R root:sftp-user /var/log/app-backup
sudo chmod 750 /var/log/app-backup
sudo chmod 640 /var/log/app-backup/backup-check.log
```

**Kiểm tra lại trên VPS:**
```bash
id sftp-user
ls -l /var/log/app-backup/backup-check.log
```
*Lưu ý chụp màn hình kết quả chạy các lệnh kiểm tra này.*

## 2. Kết nối SFTP và tải tệp tin về máy tính (Windows)

**Các bước thực hiện (Khuyên dùng Bitvise SSH Client hoặc WinSCP):**
1. Mở phần mềm SFTP Client trên máy tính Windows.
2. Điền thông tin kết nối vào VPS của bạn:
   - **Host:** `<IP_của_VPS>`
   - **Username:** `sftp-user`
   - **Password:** `<Mật_khẩu_bạn_đã_đặt>`
3. Bấm **Log in** (hoặc Login).
4. Mở cửa sổ quản lý file SFTP (chọn *New SFTP Window* nếu bạn dùng Bitvise).
5. Khung bên phải (Remote files - máy chủ): Tìm đến đường dẫn `/var/log/app-backup/`.
6. Khung bên trái (Local files - máy tính cá nhân): Tìm đến thư mục bạn muốn lưu file.
7. Kéo thả tệp `backup-check.log` từ khung bên phải sang khung bên trái (hoặc chuột phải chọn Download) để tải về máy tính.

## 3. Hình ảnh minh họa (Cần bổ sung)

*(Hãy thay thế các đoạn text bên dưới bằng hình ảnh chụp màn hình thực tế của bạn để hoàn thiện bài tập và nộp lên GitHub)*

### Ảnh 1: Tạo user và kiểm tra phân quyền thành công trên VPS
`![Tao_user_VPS](./images/anh1_vps.png)`

### Ảnh 2: Giao diện kết nối SFTP thành công và thực hiện tải file (Bitvise/WinSCP)
`![Ket_noi_SFTP](./images/anh2_sftp.png)`

### Ảnh 3: Mở và kiểm tra nội dung file backup-check.log đã được tải về máy tính
`![Kiem_tra_file_local](./images/anh3_file_local.png)`
