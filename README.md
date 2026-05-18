# Bookingcare

Bookingcare là hệ thống đặt lịch khám bệnh gồm frontend React/Vite và backend Spring Boot. Dự án phục vụ bệnh nhân, bác sĩ, nhân viên hỗ trợ và quản trị viên trong các nghiệp vụ đặt lịch, quản lý phòng khám, hóa đơn và thống kê.

## Mục lục

- [Giới thiệu](#giới-thiệu)
- [Tính năng](#tính-năng)
- [Yêu cầu hệ thống](#yêu-cầu-hệ-thống)
- [Cài đặt](#cài-đặt)
- [Hướng dẫn sử dụng](#hướng-dẫn-sử-dụng)
- [Cấu hình](#cấu-hình)
- [Đóng góp](#đóng-góp)
- [Giấy phép](#giấy-phép)

## Giới thiệu

Repo này gồm hai ứng dụng chính:

- `be_booking-care`: REST API Spring Boot cho xác thực, phân quyền, đặt lịch, phòng khám, bác sĩ, bệnh nhân, hóa đơn, phản hồi và thống kê.
- `fe_booking-care`: giao diện React/Vite cho trang đặt lịch, dashboard quản trị, dashboard bác sĩ, dashboard phòng khám và các màn hình hỗ trợ vận hành.

## Tính năng

- Xác thực và phân quyền theo vai trò `ADMIN`, `DOCTOR`, `SUPPORT`, `CLIENT`.
- Quản lý tài khoản, bệnh nhân, bác sĩ, phòng khám, chuyên khoa và dịch vụ.
- Đặt lịch khám, cập nhật trạng thái lịch hẹn và xem khung giờ trống của bác sĩ.
- Quản lý hóa đơn, hồ sơ khám bệnh, phản hồi và thông báo.
- Dashboard thống kê doanh thu và lịch đặt thành công.
- Gửi email OTP, xác thực đăng ký, quên mật khẩu và thông báo đặt/hủy lịch.
- Upload và quản lý hình ảnh qua Cloudinary.

## Yêu cầu hệ thống

- Java 17+
- MySQL 8.0+
- Node.js 20.19+ hoặc 22.12+ vì frontend dùng Vite 7
- npm 10+ hoặc phiên bản đi kèm Node tương thích
- Maven 3.6+ hoặc Maven Wrapper có sẵn trong `be_booking-care`

## Cài đặt

Clone repo:

```bash
git clone https://github.com/ndvuongq04/Bookingcare.git
cd Bookingcare
```

Cấu hình backend trong `be_booking-care/src/main/resources/application.properties`. Không commit secret thật khi dùng môi trường production.

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/booking_care?createDatabaseIfNotExist=true&useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=your_mysql_password

booking-care.jwt.base-secret=your_base64_secret

booking-care.cloudinary.cloud_name=your_cloud_name
booking-care.cloudinary.api_key=your_api_key
booking-care.cloudinary.api_secret=your_api_secret

spring.mail.username=your_email@gmail.com
spring.mail.password=your_app_password
```

Chạy backend:

```bash
cd be_booking-care
./mvnw spring-boot:run
```

Trên Windows PowerShell:

```powershell
cd be_booking-care
.\mvnw.cmd spring-boot:run
```

Chạy frontend ở một terminal khác:

```bash
cd fe_booking-care
npm install
npm run dev
```

Mặc định:

- Backend: `http://localhost:8080`
- API base path: `http://localhost:8080/api/v1`
- Frontend: `http://localhost:5173`
- Database: `booking_care`

## Hướng dẫn sử dụng

Sau khi backend chạy lần đầu, `DatabaseInitialize` tự tạo các vai trò, một tài khoản admin mặc định, danh sách khung giờ và danh sách tỉnh/thành.

Tài khoản admin mặc định:

```text
Email: superAdmin01@gmail.com
Password: 123456
```

Đăng nhập qua API:

```bash
curl -X POST http://localhost:8080/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"superAdmin01@gmail.com","password":"123456"}'
```

Lấy danh sách bác sĩ công khai:

```bash
curl "http://localhost:8080/api/v1/doctors?page=1&size=10"
```

Build frontend trước khi deploy:

```bash
cd fe_booking-care
npm run build
npm run preview
```

## Cấu hình

Các file cấu hình chính:

- Backend: `be_booking-care/src/main/resources/application.properties`
- Frontend API client: `fe_booking-care/src/api/axios.ts`
- Frontend API constant: `fe_booking-care/src/utils/constant.ts`
- CORS backend: `be_booking-care/src/main/java/com/Booking_care/config/CorsConfig.java`
- Security rules: `be_booking-care/src/main/java/com/Booking_care/config/SecurityConfig.java`

Nếu đổi host hoặc port backend, cập nhật các URL `localhost:8080` trong frontend để trỏ đúng API.
