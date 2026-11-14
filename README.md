# Hệ Thống Đặt Món & Quản Lý Quán Hủ Tiếu

Ứng dụng web đặt món tại bàn qua mã QR và quản lý bếp/doanh thu theo thời gian thực (Realtime).

## 1. Cấu trúc hệ thống
* **`Khach.html`**: Giao diện gọi món dành cho khách hàng (quét QR).
* **`QuanLy.html`**: Ứng dụng quản lý tích hợp 3 chức năng:
  * **Bếp (Trang chính)**: Nhận đơn mới tự động, đọc tên món và ghi chú bằng giọng nói tiếng Việt (Web Speech API).
  * **Menu**: Thêm/sửa giá, bật tắt trạng thái còn/hết món, sắp xếp thứ tự hiển thị theo thời gian thực.
  * **Thống kê**: Tự động tổng hợp doanh thu ngày/tháng/năm và lưu lịch sử 50 đơn gần nhất.

## 2. Công nghệ sử dụng
* **Frontend**: HTML5, CSS3, Vanilla JavaScript (mô hình Single Page Application).
* **Backend & Database**: Supabase (PostgreSQL, Realtime Subscriptions, `pg_cron`).
* **Hosting**: Vercel / Netlify.

## 3. Quy ước đường dẫn mã QR
* **Khách ngồi tại bàn**: `https://<ten-mien-cua-ban>/Khach.html?table=1` (thay số `1` bằng số bàn tương ứng).
* **Khách mua mang về**: `https://<ten-mien-cua-ban>/Khach.html` (không kèm tham số `table`).

## 4. Vận hành & Bảo trì Database
* **Cài đặt trên thiết bị của quán**: Mở `QuanLyChung.html` trên Chrome/Safari -> Chọn **Thêm vào màn hình chính (Add to Home Screen)** -> Chỉnh màn hình luôn sáng và chạm vào màn hình 1 lần mỗi sáng để kích hoạt giọng đọc thông báo đơn.
* **Tự động dọn dẹp dữ liệu**: Hệ thống đã cấu hình `pg_cron` chạy vào `00:00` ngày mùng 1 hàng tháng để tự động xóa các đơn hàng đã phục vụ quá 2 năm.
* **Lệnh xóa dữ liệu test trước khi bàn giao**:
  ```sql
  TRUNCATE TABLE order_items, orders RESTART IDENTITY CASCADE;
