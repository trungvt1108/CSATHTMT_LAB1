# Báo cáo Lab S1: Mô hình đe dọa hệ thống bán hàng trực tuyến

## 1. Hệ thống lựa chọn
Hệ thống được chọn để lập mô hình đe dọa là một **Trang bán hàng trực tuyến** cơ bản, bao gồm đúng 3 thành phần cốt lõi:
- **Trình duyệt (Client):** Giao diện để khách hàng xem sản phẩm, gửi yêu cầu và nhận hiển thị từ máy chủ.
- **Máy chủ ứng dụng (Web Server):** Xử lý logic nghiệp vụ, xác thực người dùng, phân quyền và kết nối cơ sở dữ liệu.
- **Cơ sở dữ liệu (Database):** Nơi lưu trữ toàn bộ thông tin nhạy cảm của tài khoản, mật khẩu và lịch sử đơn hàng.

## 2. Ba mối đe dọa ưu tiên xử lý
Dựa trên danh sách 8 mối đe dọa đã phân tích, 3 mối đe dọa sau được ưu tiên chọn để xử lý:

*   **M03 (Brute Force):** Kẻ tấn công dò đoán thành công mật khẩu của tài khoản quản trị trong trường hợp máy chủ không giới hạn số lần đăng nhập sai liên tiếp. *(Tác động: 5, Khả năng: 4. Điểm rủi ro: 20)*
*   **M01 (IDOR):** Người dùng đã đăng nhập với vai trò khách hàng đọc được bảng đơn hàng của khách hàng khác nếu ứng dụng không lọc truy cập theo mã tài khoản người dùng. *(Tác động: 4, Khả năng: 4. Điểm rủi ro: 16)*
*   **M02 (SQL Injection):** Kẻ tấn công trích xuất được toàn bộ danh sách tài khoản quản trị khi ứng dụng không làm sạch dữ liệu đầu vào tại ô tìm kiếm sản phẩm. *(Tác động: 5, Khả năng: 3. Điểm rủi ro: 15)*

## 3. Lập luận lựa chọn
Quyết định chọn 3 mối đe dọa trên được đưa ra dựa trên tích số rủi ro (Tác động x Khả năng) cao nhất đối với hệ thống, tập trung vào việc bảo vệ tính bảo mật của dữ liệu khách hàng và quyền quản trị cốt lõi. 

Cụ thể, **M03 (20 điểm)** và **M02 (15 điểm)** trực tiếp đe dọa đến quyền kiểm soát cao nhất của hệ thống. Nếu kẻ tấn công chiếm được tài khoản quản trị hoặc trích xuất được cơ sở dữ liệu, toàn bộ hệ thống sẽ sụp đổ. **M01 (16 điểm)** là lỗ hổng logic nghiêm trọng ở tầng ứng dụng, xâm phạm trực tiếp đến quyền riêng tư và dữ liệu đơn hàng của khách, gây hậu quả pháp lý và ảnh hưởng nghiêm trọng đến uy tín doanh nghiệp.

**Thừa nhận các rủi ro bị bỏ lại:**
Để tập trung nguồn lực kỹ thuật xử lý triệt để 3 lỗ hổng chí mạng ở tầng logic ứng dụng, tôi chấp nhận tạm gác lại 5 mối đe dọa còn lại. Đáng chú ý là rủi ro tấn công quá tải DoS (M08) và đánh cắp phiên đăng nhập (M05). Tuy nhiên, việc chống DoS (M08) có thể được giảm nhẹ một phần nhờ hạ tầng mạng (như tường lửa WAF/Cloudflare) thay vì sửa mã nguồn. Các lỗi như XSS (M04) hay quên đổi mật khẩu mặc định (M07) có thể được kiểm soát thông qua các quy trình rà soát cấu hình định kỳ trong tương lai.