# Danh sách lớp v0 — Hoàng Quang Quyền

**MSSV** 2A202603082 · **Mã bộ ảnh** G07

Bản nháp dựa trên Guideline Cabin v1.0. Cột IR mô tả khả năng quan sát dự kiến theo từng nguồn ảnh.

## DMS · NIR thật (Drive&Act)

| # | Lớp / thuộc tính | Giá trị | Để làm gì | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `face` | Box mặt nhìn thấy | Xác định vùng mặt để theo dõi mắt và tư thế đầu qua các khung. | Có nếu đủ chi tiết vùng mặt. |
| 2 | `face.eye_left`, `face.eye_right` | `open`, `closed`, `unknown` cho từng mắt | Theo dõi thời lượng nhắm mắt qua chuỗi khung để ước lượng mức tỉnh táo. | Có khi thấy khe mắt hoặc hai mí. Dùng `unknown` nếu tròng kính hay phản xạ che mắt. |
| 3 | `face.gaze_zone` | `road`, `mirror_l`, `mirror_r`, `mirror_c`, `cluster`, `center`, `lap`, `other` | Theo dõi thời lượng và vị trí tài xế rời mắt khỏi đường. | Có thể suy từ mắt và hướng đầu khi thấy rõ. Dùng `other` và ghi lý do nếu chưa xác định được vùng. |
| 4 | `face.glasses` | `none`, `clear`, `sun`, `unknown` | Nhận diện điều kiện làm giảm độ tin cậy của nhãn mắt và vùng nhìn. | Gọng có thể thấy rõ. Kính râm có thể trông trong suốt dưới NIR, nên dùng `unknown` nếu chưa phân loại được. |
| 5 | `face13` | 13 điểm, cờ `occluded` / `outside` theo từng điểm | Ước lượng tư thế đầu và định vị mí mắt để hỗ trợ theo dõi mắt. | Có khi mặt đủ rộng và thấy chi tiết. Đánh cờ theo mức độ che khuất. |
| 6 | `hand` | `whose`: `driver`, `passenger` · `side`: `left`, `right` · `on_wheel`: `yes`, `no`, `unknown` | Theo dõi mức sẵn sàng thao tác với vô lăng theo chế độ lái. | Có khi thấy tay. `on_wheel` phụ thuộc vào việc nhìn rõ vùng tiếp xúc với vô lăng. |
| 7 | `phone.state` | `held`, `mounted`, `lap` | Phân biệt điện thoại cầm tay với điện thoại trên giá hay trên đùi để nhận diện tình huống có nguy cơ mất tập trung. | Có khi thấy một cạnh, góc hoặc màn hình đủ rõ để nhận dạng. |
| 8 | `person.seatbelt` | `worn_correct`, `worn_incorrect`, `not_worn`, `unknown` | Nhắc tài xế thắt dây hoặc chỉnh dây về đúng vị trí. | Phụ thuộc vào độ tương phản và việc nhìn thấy đường dây qua vai, ngực và hông. |

## OMS · RGB và IR mô phỏng (SVIRO)

| # | Lớp / thuộc tính | Giá trị | Để làm gì | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person.seat` | `rear_left`, `rear_middle`, `rear_right` trên bộ OMS | Xác định vị trí hành khách để đếm người theo ghế và gắn cảnh báo với đúng vị trí. | Thường có khi thấy đường nét cơ thể và vị trí ghế. |
| 2 | `person.seatbelt` | `not_worn`, `unknown` trên SVIRO | Cung cấp trạng thái dây theo ghế cho chức năng nhắc thắt dây. Bộ ảnh này chỉ có dữ liệu cho hai giá trị trên. | Có thể thấy phần thân và vùng dây nếu đủ tương phản. Dùng `unknown` khi vùng dây bị che hoặc quá tối. |
| 3 | `child_seat` | Box theo ghế · `occupied`: `no`, `unknown` | Nhận diện vị trí lắp ghế trẻ em và cho biết khi nào nhìn đủ rõ để xác nhận ghế trống. | Hình dạng và thành ghế có thể thấy được. Lòng ghế có thể bị che. |
| 4 | `infant_seat` | Box theo ghế · `facing`: `rear`, `front` | Xác định vị trí nôi và hướng đặt để hỗ trợ kiểm tra cách lắp. | Có thể xác định khi thấy mép, đế hoặc hướng mở của nôi. |
| 5 | `object` | Box theo ghế · `type`: `bag`, `box`, `cup`, `other` | Nhận diện đồ để trên ghế, hỗ trợ phân biệt với hành khách và kiểm tra đồ bỏ quên. | Dựa vào hình dạng. Dùng `other` nếu chỉ xác định được đó là đồ vật. |
| 6 | Ghế trống | Ghi chú theo ghế khi quan sát đủ rõ | Đếm ghế còn trống và tránh tính nhầm ghế trống là có người. | Có khi thấy rõ mặt ghế và khoảng không phía trên. Guideline yêu cầu ghi chú, không vẽ box. |
| 7 | `frame_meta.camera_blocked` | `yes`, `no` | Xác định khung cần kiểm tra lại trước khi dùng kết quả nhận diện theo ghế. | Có thể thấy vùng bị che khi vật chắn chiếm trường nhìn. |

## Hai câu hỏi cho coach

1. **DMS.** Với ảnh D## xoay 90°, ngưỡng 60 px ở §4.6 đo theo chiều ngang của box trong file hay theo bề ngang giải phẫu của mặt? **Khung ví dụ** `D01`, box mặt trong bản nháp có chiều x khoảng 238 px và chiều y khoảng 239 px. Cả hai vượt ngưỡng ở khung này. **Cách xử lý tạm** dùng `xbr − xtl` theo `cabin_check` và hỏi lại nếu gặp ca có hai số đo nằm ở hai phía của ngưỡng.
2. **OMS.** Nếu nhận ra `infant_seat` nhưng không đủ dấu hiệu để phân biệt `rear` với `front`, cần ghi `facing` thế nào khi schema chỉ có hai giá trị? **Khung ví dụ** `S05_ir`, bản nháp đề xuất `infant_seat` ở `rear_right` với `facing=rear`. Thông tin hình dạng trên IR-sim chưa đủ để xác nhận hướng. **Cách xử lý tạm** ghi ca khó và nhờ coach xác nhận hướng trước khi chốt thuộc tính.
