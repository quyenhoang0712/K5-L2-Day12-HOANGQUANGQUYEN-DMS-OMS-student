# Khai báo sử dụng AI — Hoàng Quảng Quyền

| # | Công cụ | Mục đích sử dụng | Kết quả áp dụng | Cách kiểm tra |
|---|---|---|---|---|
| 1 | ChatGPT Codex | Phân tích yêu cầu Day 12, hỗ trợ thiết lập môi trường và chuẩn bị dữ liệu. | Chuẩn bị CVAT local, cấu trúc nhãn và hồ sơ bài làm. | Đối chiếu `GUIDE.md`, schema và `SUBMISSION.md`. |
| 2 | ChatGPT Codex, YOLO, Qwen3-VL, Apple Vision, CLIP | Hỗ trợ nhận diện đối tượng, gán nhãn và điều chỉnh thuộc tính. | Tạo và cập nhật nhãn cho bộ ảnh G07, bao gồm các phiên bản export. | Kiểm tra XML, cấu trúc nhãn và chạy `cabin_check`. |
| 3 | ChatGPT Codex, MediaPipe, YOLOE, YOLOWorld | Rà soát nhãn, điểm mặt, tay và đối chiếu kết quả. | Cập nhật bản V2, bảng đo DMS, `disagreement_log.csv` và phần mở rộng. | Sử dụng IoU, kiểm tra tọa độ và `cabin_check diff`. |
| 4 | ChatGPT Codex | Tổng hợp kết quả và hoàn thiện tài liệu nộp. | Chuẩn bị `self_review.md`, báo cáo, các bảng kiểm tra và ZIP. | Đối chiếu dữ liệu trong CSV, XML và cấu trúc hồ sơ. |

## Ghi chú

Các công cụ AI được sử dụng trong quá trình chuẩn bị dữ liệu, gán nhãn, rà soát và hoàn thiện bài Lab Day 12. Dữ liệu ảnh được xử lý trên môi trường local.

Kết quả được kiểm tra bằng các công cụ đánh giá và đối chiếu phiên bản. Phần E3 được đối chiếu bằng AI, không phải đánh giá độc lập từ thành viên khác.