# Bài 1: Sự cố không truy cập được hệ thống quản trị

## Phần 1. Phân tích

### 1. Tiến trình DNS Lookup

- **Input:** `phong-dieu-phoi.greenlog.vn`
- **Output:** Địa chỉ IP của máy chủ.

**Các bước xử lý:**

1. Người dùng nhập tên miền vào trình duyệt.
2. Máy tính kiểm tra DNS Cache xem đã lưu IP chưa.
3. Nếu chưa có, gửi yêu cầu đến DNS Resolver.
4. DNS Resolver tìm kiếm qua Root DNS, TLD DNS và Authoritative DNS.
5. DNS trả về địa chỉ IP để trình duyệt kết nối đến máy chủ.

### 2. Phân biệt lỗi DNS và lỗi HTTP 500

| Lỗi DNS | Lỗi HTTP 500 |
|---|---|
| Không tìm thấy tên miền. | Máy chủ gặp lỗi nội bộ. |
| Xảy ra trước khi kết nối HTTP đến Server. | Server đã nhận yêu cầu. |
| Liên quan đến DNS hoặc tên miền. | Liên quan đến Server. |

**Kết luận:** Lỗi NXDOMAIN không có nghĩa là máy chủ bị sập mà có thể do nhập sai tên miền.

## Phần 2. Triển khai

### Quy trình kiểm tra HTTP Status Code

1. Mở trình duyệt và nhấn `F12`.
2. Chọn tab **Network**.
3. Tải lại trang web.
4. Kiểm tra cột **Status** để xem mã phản hồi.

Một số mã thường gặp:

- `200`: Truy cập thành công.
- `301/302`: Chuyển hướng.
- `404`: Không tìm thấy trang.
- `500`: Máy chủ gặp lỗi.
- `503`: Máy chủ tạm thời không hoạt động.

### Xử lý lỗi tên miền

Nam nhập sai `grenlog.vn` thay vì `greenlog.vn` (thiếu chữ e). DNS sẽ tìm kiếm một tên miền khác trong danh bạ Internet. Nếu không có bản ghi tương ứng thì trả về lỗi NXDOMAIN.

**Cách xử lý:** Nhập lại đúng tên miền.

## Phần 3. Kiểm thử

Nếu Nam sửa đúng tên miền nhưng vẫn nhận mã `404 Not Found` thì:

- Tên miền đã được phân giải thành công.
- Trình duyệt đã gửi yêu cầu đến máy chủ.
- Máy chủ không tìm thấy trang hoặc tài nguyên được yêu cầu.

**Nguyên nhân:** Sai đường dẫn URL, trang đã bị xóa hoặc chưa được triển khai.

**Cách khắc phục:** Kiểm tra lại đường dẫn và cấu hình trang trên Server.
