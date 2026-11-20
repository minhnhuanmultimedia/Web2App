# Web2App

Biến website hoặc file `index.html` thành app Android (APK/AAB) bằng GitHub Actions. Không cần cài gì trên máy.

## Cách dùng (3 bước)

1. **Chọn nguồn nội dung**
   - Có sẵn website: không cần làm gì, lát nữa nhập link.
   - Muốn đóng gói file riêng: tải `index.html` (và các file đi kèm) lên repo. Nếu để trong thư mục `web/` thì chỉ thư mục này được đóng gói.
   - (Tuỳ chọn) tải `icon.png` vuông 512x512 lên repo làm biểu tượng app. Không có thì app tự dùng chữ cái đầu của tên app.
2. Vào tab **Actions** > **Build Android APK** > **Run workflow**.
3. Điền form rồi bấm **Run workflow**. Khoảng 5 đến 10 phút sau, mở lần chạy đó, kéo xuống mục **Artifacts** để tải `android-app`.

## Các ô trong form

| Ô | Bắt buộc | Ý nghĩa | Để trống thì |
|---|---|---|---|
| Tên app | Có | Tên hiện dưới biểu tượng app (tối đa 30 ký tự) | |
| Link website | Không | Địa chỉ `https://...` để app mở | Đóng gói `index.html` trong repo |
| Package ID | Không | Mã định danh app, ví dụ `com.tenban.cuahang` | Tự tạo `com.web2app.<tên repo>` |
| Version code | Không | Số nguyên, phải tăng mỗi lần cập nhật app | Tự dùng số thứ tự lần build |
| Build AAB | Không | Tick nếu muốn đưa app lên Google Play | Chỉ build APK |

## Khoá ký app (quan trọng nếu cập nhật app hoặc đăng Google Play)

- **Lần build đầu**: workflow tự tạo khoá ký mới và xuất artifact `keystore-backup` (chứa khoá và mật khẩu).
- **Để các lần sau dùng cùng khoá**: vào *Settings > Secrets and variables > Actions*, tạo 2 secret `KEYSTORE_BASE64` (nội dung file `keystore-base64.txt`) và `KEYSTORE_PASSWORD` (mật khẩu trong `keystore-info.txt`).
- Mất khoá hoặc đổi khoá thì không thể cập nhật app đã đăng. Hãy sao lưu cẩn thận và không chia sẻ artifact `keystore-backup`.

## Lỗi thường gặp

| Thông báo | Cách xử lý |
|---|---|
| `Repo chưa có index.html` | Tải `index.html` lên repo hoặc nhập link website |
| `Package ID không hợp lệ` | Dùng chữ thường không dấu, có ít nhất 2 phần ngăn bởi dấu chấm |
| `Keystore, alias hoặc mật khẩu không khớp` | Kiểm tra lại 2 Secrets, alias phải là `upload` |
