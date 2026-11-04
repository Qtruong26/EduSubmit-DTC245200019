# Bài 1 – Tổng quan hệ thống và MEP

Họ tên: Nguyen Van A MSSV: DTC245200019 Lớp: ...

## 1. Cài đặt

- Đã cài đặt thành công .NET 10 SDK, VS Code (C# Dev Kit, REST Client) và Git.

## 2. MepDemo (Bước 7)

- MEP 3: Mỗi bài nộp sub-x chỉ do 1 worker xử lý (các worker chia nhau việc).
- MEP 4: Một sự kiện được gửi đồng thời tới cả 3 dịch vụ.
- Mô tả các MEP:
  - Request-Response: Client gửi yêu cầu và đợi Server phản hồi trực tiếp.
  - One-Way: Client gửi tin nhắn đi và không chờ phản hồi.
  - Queue-Based: Gửi thông điệp vào hàng đợi để các worker luân phiên nhận xử lý.
  - Publish-Subscribe: Phát sự kiện tới tất cả các dịch vụ đăng ký lắng nghe.
  - Streaming: Truyền luồng dữ liệu liên tục giữa Client và Server.

## 3. BasicWebApi (Bước 8)

- Đã chạy và kiểm thử 5 request trong REST Client (trả về đúng 200, 201, 400).
- Trả lời 8.4:
  1. `builder.Services.AddSingleton...` phải đứng trước `var app = builder.Build();` để đăng ký dịch vụ vào DI Container trước khi ứng dụng khởi tạo.
  2. `name` lấy từ Route parameter (`/hello/{name}`), `language` lấy từ Query parameter (`?language=en`).
  3. POST trả về `201 Created` vì đây là chuẩn RESTful khi tạo mới thành công một tài nguyên (kèm vị trí ở header `Location`).

## 4. MiniDistributedSystem (Bước 9)

- Trả lời:
  1. Client gọi `GET /api/assignments` (200 OK) và `POST /api/submissions` (201 Created).
  2. Client và API không dùng chung code, thống nhất qua chuẩn HTTP và định dạng JSON.
  3. Truyền địa chỉ qua tham số để linh hoạt thay đổi server mà không cần biên dịch lại code.

## 5. Thí nghiệm lỗi (Bước 10)

| #   | Dự đoán                     | Thực tế                              | Đúng/Sai | Giải thích                                                    |
| --- | --------------------------- | ------------------------------------ | -------- | ------------------------------------------------------------- |
| a   | Client vẫn chạy bình thường | In ra đủ 3 bài tập                   | Đúng     | Client xử lý danh sách động JSON nên không bị cứng code       |
| b   | Client báo lỗi              | Ném ngoại lệ `HttpRequestException`  | Đúng     | Giao tiếp qua mạng thất bại do máy chủ không lắng nghe        |
| c   | Server từ chối              | Trả mã `400 Bad Request`             | Đúng     | Server cần validate dữ liệu để tránh lưu dữ liệu bẩn          |
| d   | Đổi cổng chạyClient         | Client chạy thành công với cổng 5050 | Đúng     | Truyền cổng mới qua tham số command-line khi khởi chạy Client |

## 6. Dự án EduSubmit tự làm (Bước 11-12)

- Hoàn thành đầy đủ API, Client và Endpoint mở rộng `GET /api/submissions/{id}` (200 OK / 404 Not Found).
- Luồng thông điệp:
  - Client --(HTTP GET /api/assignments)--> Api : Request-Response
  - Client --(HTTP POST /api/submissions)--> Api : Request-Response (Trả về 201 Created)
  - Client --(HTTP GET /api/submissions/{id})--> Api : Request-Response (Đọc lại lượt nộp)

## 7. Khó khăn gặp phải và cách giải quyết

- Nhầm lẫn khi gõ lệnh PowerShell làm lỗi đường dẫn -> Giải quyết: Xóa tạo lại đúng cú pháp thư mục.
