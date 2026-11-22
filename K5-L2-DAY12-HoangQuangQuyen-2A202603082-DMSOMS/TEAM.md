# TEAM — Day 12

**Mã bộ ảnh:** G07  
**Hình thức:** Nhóm 2 thành viên  
**Tổng khối lượng:** 22 khung ảnh cốt lõi

## 1. Thông tin nhóm

| Họ tên | MSSV | Số khung phụ trách | Bạn review chéo | Vai trò |
|---|---|---|---|---|
| Hoàng Quang Quyền | 2A202603082 | 11/22 khung | Hồ Đăng Mạnh | Gán nhãn, kiểm tra DMS, tổng hợp hồ sơ |
| Hồ Đăng Mạnh | 2A202603092 | 11/22 khung | Hoàng Quang Quyền | Gán nhãn, rà soát OMS, đối chiếu RGB/IR |

**Tổng tiến độ gán nhãn:** 22/22 khung.

**Kết quả đối chiếu chung:** 30 dòng trong `disagreement_log.csv`.

## 2. Nội dung công việc

### 2.1. Hoàng Quang Quyền — 2A202603082

**Phần việc chính**

- Phụ trách gán nhãn 11 khung ảnh thuộc bộ dữ liệu G07 trên CVAT local.
- Tập trung kiểm tra nhóm nhãn DMS, bao gồm khuôn mặt, bàn tay, điểm đặc trưng và các thuộc tính liên quan.
- Phụ trách tổng hợp cấu trúc nhãn, kiểm tra định dạng và tính hợp lệ của dữ liệu xuất từ CVAT.
- Theo dõi các phiên bản nhãn, đối chiếu thay đổi về bounding box, thuộc tính và tọa độ.
- Tổng hợp bảng đo DMS, ghi chú tự rà và các trường hợp chưa thống nhất trong `self_review.md`.
- Phụ trách tổng hợp các kết quả kiểm tra và chuẩn bị hồ sơ nộp bài.

**Kiểm soát chất lượng**

Tập trung kiểm tra số lượng đối tượng, vị trí bounding box, trạng thái che khuất và độ chính xác của các điểm đặc trưng khuôn mặt.

Các kết quả được đối chiếu bằng XML, IoU và `cabin_check`. Những vị trí chưa đủ cơ sở xác nhận được ghi nhận để tiếp tục rà soát.

**Kiến thức và kinh nghiệm rút ra**

- Hiểu rõ hơn quy tắc xác định các điểm đặc trưng khuôn mặt và bàn tay.
- Củng cố cách xử lý đối tượng bị che khuất hoặc không đủ thông tin hình ảnh.
- Nắm được cách đối chiếu tọa độ, thuộc tính và kiểm tra tính nhất quán giữa các phiên bản nhãn.
- Rút kinh nghiệm về việc ghi nhận trường hợp chưa chắc chắn thay vì tự ý kết luận.

### 2.2. Hồ Đăng Mạnh — 2A202603092

**Phần việc chính**

- Phụ trách gán nhãn 11 khung ảnh còn lại thuộc bộ dữ liệu G07 trên CVAT local.
- Trọng tâm phân công là các nhãn OMS, bao gồm người ngồi, ghế trẻ em, vật thể và vị trí tương ứng trong cabin.
- Phối hợp rà soát sự khác biệt giữa ảnh RGB và IR.
- Kiểm tra cách xác định vị trí ghế theo hướng phương tiện, phân biệt loại ghế và những đối tượng có khả năng bị che khuất.
- Phối hợp đối chiếu các trường hợp có nhãn thiếu, nhãn trùng hoặc thuộc tính chưa thống nhất.
- Tham gia tổng hợp các trường hợp cần xác minh và trao đổi kết quả trong nhóm.

**Kiểm soát chất lượng**

Trọng tâm kiểm tra gồm tính nhất quán của lớp nhãn, vị trí ghế, ranh giới đối tượng và thuộc tính trong các ảnh cabin.

Các trường hợp hình ảnh thiếu chi tiết hoặc khó xác định đối tượng cần được ghi nhận riêng để phục vụ đối chiếu.

**Kiến thức trọng tâm**

- Phân biệt các lớp đối tượng trong cabin, đặc biệt là các loại ghế trẻ em.
- Xác định vị trí trái, phải và giữa dựa trên hệ quy chiếu của phương tiện.
- Đánh giá ảnh RGB và IR khi nhận diện vật thể.
- Sử dụng kết quả đối chiếu để phát hiện những vị trí cần kiểm tra lại.

## 3. Phối hợp và kiểm soát chất lượng

### Phân chia công việc

| Hạng mục | Hoàng Quang Quyền | Hồ Đăng Mạnh |
|---|---|---|
| Gán nhãn dữ liệu G07 | 11 khung | 11 khung |
| Trọng tâm rà soát | DMS, mặt, tay | OMS, ghế, vật thể |
| Đối chiếu RGB/IR | Phối hợp | Trọng tâm |
| Kiểm tra tọa độ và thuộc tính | Trọng tâm DMS | Trọng tâm OMS |
| Tổng hợp báo cáo | Phụ trách chính | Phối hợp |
| Kiểm tra chéo | Phân công với Mạnh | Phân công với Quyền |

### Quy trình kiểm tra

Dữ liệu được quản lý trên CVAT local và xuất sang định dạng XML để kiểm tra cấu trúc nhãn.

Các bước đánh giá kỹ thuật gồm:

1. Đối chiếu lớp nhãn và thuộc tính theo guideline.
2. Kiểm tra tọa độ, vị trí bounding box và điểm đặc trưng.
3. So sánh các phiên bản bằng `cabin_check diff`.
4. Ghi nhận các trường hợp bất đồng trong `disagreement_log.csv`.
5. Tổng hợp kết quả và những trường hợp chưa chắc chắn trong `self_review.md`.

Lượt đối chiếu E3 sử dụng mô hình thị giác máy tính chạy local. Kết quả này được ghi nhận riêng với hoạt động review trực tiếp của thành viên.

## 4. Các trường hợp cần xác minh

| Khung ảnh | Nội dung cần xem xét |
|---|---|
| `S01` | Xác định đối tượng có đặc điểm giống trẻ nhỏ |
| `S06` | Phân biệt loại ghế và hướng đặt ghế |
| `S08` | Xác định đối tượng người và ghế trẻ em |
| `D06` | Kiểm tra vị trí các điểm đặc trưng khuôn mặt |
| `S09`, `S10` | Đối chiếu thêm với dữ liệu gốc của phần mở rộng |

Các trường hợp trên được ghi nhận trong hồ sơ và chưa có xác nhận cuối cùng từ coach.

## 5. Công cụ và phương pháp hỗ trợ

Nhóm sử dụng CVAT local để quản lý dữ liệu gán nhãn; ChatGPT Codex và các mô hình thị giác máy tính chạy local để hỗ trợ chuẩn bị dữ liệu, gán nhãn, đối chiếu và kiểm tra kết quả.

Các phiên bản dữ liệu được đánh giá thông qua XML, `cabin_check`, `cabin_check diff` và các phép đo hình học.

Chi tiết việc sử dụng công cụ được lưu trong `prompts.md`.

## 6. Tổng kết nhóm

Nhóm G07 gồm hai thành viên Hoàng Quang Quyền và Hồ Đăng Mạnh, với khối lượng gán nhãn được phân chia đồng đều, mỗi thành viên phụ trách 11/22 khung ảnh.

**Kết quả chung**

- Hoàn thành phạm vi gán nhãn 22/22 khung ảnh cốt lõi.
- Có các phiên bản dữ liệu phục vụ kiểm tra và đối chiếu.
- Ghi nhận 30 dòng trong `disagreement_log.csv`.
- Có bảng đo DMS và ghi chú tự rà trong `self_review.md`.
- Hồ sơ có thêm phần mở rộng gồm 4 khung thuộc `S09`, `S10`.
- Những trường hợp chưa thống nhất được ghi nhận để tiếp tục xác minh.

**Kết luận**

Nhóm đã hoàn thành khối lượng gán nhãn cốt lõi theo phân chia 11 khung mỗi thành viên, xây dựng hồ sơ kiểm tra và ghi nhận các vấn đề cần rà soát. Quá trình thực hiện giúp củng cố kiến thức về DMS, OMS, quản lý dữ liệu, kiểm tra chất lượng và phối hợp công việc trong nhóm.

Kết quả kiểm tra kỹ thuật được phân biệt với đánh giá chất lượng nhãn cuối cùng và các trường hợp còn chờ xác nhận.