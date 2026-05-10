# Service Boundary của nhóm 10

## 1. Thông tin nhóm

- Tên nhóm: 10
- Lớp: CNTT17-09
- Thành viên: Trịnh Việt Đức (1771020167)
- Service nhóm phụ trách: AI Vision Service
- Sản phẩm tổng thể của lớp: Smart Campus Operations Platform - Product A

## 2. Actor

Các actor/sender chính tương tác với service này gồm:

- Camera Stream Service, gửi ảnh hoặc frame cần phân tích.
- Core Business Service, nhận kết quả phát hiện để ra quyết định nghiệp vụ.
- Analytics Service, lấy dữ liệu phát hiện để tổng hợp thống kê.
- Người phát triển hoặc kiểm thử, gọi API trực tiếp để xác minh service.

## 3. System Boundary

Nhóm 10 xây phần AI Vision trong Product A.

Phần nhóm kiểm soát:

- Nhận ảnh hoặc URL ảnh từ Camera Stream Service.
- Phân tích ảnh bằng AI hoặc mô phỏng AI.
- Trả kết quả phát hiện, mức độ tin cậy và mức rủi ro.
- Cung cấp API và tài liệu hợp đồng cho nhóm khác tích hợp.

Phần nhóm chỉ tích hợp:

- Camera Stream Service là nguồn ảnh đầu vào chính.
- Core Business Service là nơi nhận kết quả để quyết định cảnh báo.
- Analytics Service dùng dữ liệu đầu ra để thống kê.

## 4. Service Boundary

Service của nhóm có trách nhiệm phân tích hình ảnh, phát hiện đối tượng hoặc người, xác định độ tin cậy và trả kết quả cho các service downstream.

Service KHÔNG làm gì:

- Không quản lý camera vật lý hoặc luồng video dài hạn.
- Không tự gửi thông báo cuối cùng cho người dùng.
- Không thay thế Core Business trong việc ra quyết định nghiệp vụ.
- Không lưu trữ dữ liệu ảnh lớn như một kho media lâu dài.

## 5. Input / Output

### Input

- `camera_id`: mã camera hoặc nguồn ảnh.
- `image_url` hoặc dữ liệu frame ảnh.
- `timestamp`: thời điểm ảnh được gửi tới service.

### Output

- `detected`: có phát hiện hay không.
- `object`: đối tượng được phát hiện, ví dụ `person`.
- `confidence`: độ tin cậy của kết quả.
- `risk_level`: mức rủi ro để Core Business sử dụng.

## 6. API dự kiến

| Method | Endpoint | Mục đích |
|---|---|---|
| GET | /health | Kiểm tra service |
| POST | /analyze | Phân tích ảnh hoặc frame đầu vào |
| GET | /models | Kiểm tra mô hình hoặc cấu hình AI đang dùng |

## 7. Phụ thuộc service khác

Service này gọi đến service nào?

- Có thể gọi Camera Stream Service nếu cần lấy frame từ luồng camera.
- Có thể ghi log hoặc xuất dữ liệu cho Analytics Service.
- Có thể gửi kết quả sang Core Business Service để ra quyết định cảnh báo.

Service nào gọi đến service này?

- Camera Stream Service gọi sang AI Vision Service khi phát hiện frame cần phân tích.
- Core Business Service có thể gọi trực tiếp để xác thực lại kết quả AI.
- Analytics Service có thể đọc dữ liệu đầu ra hoặc event từ service này.

## 8. Sơ đồ minh họa

Có thể vẽ bằng Mermaid, draw.io, Ludichart hoặc ảnh chụp sơ đồ.

```mermaid
flowchart LR
    Camera[Camera Stream Service] --> AISvc[AI Vision Service]
    AISvc --> Core[Core Business Service]
    AISvc --> Analytics[Analytics Service]
    Operator[Tester / Developer] --> AISvc
