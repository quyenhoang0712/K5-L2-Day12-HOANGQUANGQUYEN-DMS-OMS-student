# Tự đánh giá — Hoàng Quang Quyền · G07

## 1. Tự rà soát 4 cảnh

Bảng ghi nhận các nhãn còn cần xác minh trong bản export. Các mục chưa chắc chắn cần đối chiếu trên CVAT.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| `S04_ir` | Ghế và nôi hàng sau | Có thể `wrong_class` | Bổ sung đề xuất `infant_seat` ở `rear_right` và `child_seat` ở `rear_left`. Cần kiểm tra tay xách, hướng nôi và biên ghế vì các lượt định vị cho kết quả chưa thống nhất. | Đã bổ sung nhãn nháp |
| `S08_ir` | `face` của hành khách ở `rear_left` | Có thể `missing` | Bản nháp nhận ra hai người nhưng chỉ có một box mặt. Khung RGB cùng cảnh có hai box mặt. Kiểm tra khả năng nhìn thấy mặt của hành khách ở `rear_left` trực tiếp trên IR-sim. | Chờ đối chiếu ảnh |
| `D01` | `face13` và mắt | `ok` về quy ước trục | Tọa độ y của `left_eye_outer` là 503,3 px và của `right_eye_outer` là 390,6 px. Hai thuộc tính mắt đang là `unknown`, các điểm mắt có cờ `occluded`. | Đã kiểm tra trục và cờ |
| `D06` | Mặt tài xế | Có thể `box` hoặc `flag` | Hai lượt định vị khuôn mặt trên vùng cắt cho box tương tự nhau. Các lượt định vị điểm mặt cho kết quả tương đối ổn định với hai box đầu vào. Bổ sung `face` và `face13`, giữ hai mắt `unknown` với cờ `occluded` ở điểm mắt. | Đã bổ sung nhãn nháp |

## 2. Lỗi mình lặp lại nhiều nhất

- Mục cần rà nhiều nhất là biên box và loại đối tượng khi vùng ảnh có ít chi tiết, như mặt trên `S08_ir` và nôi trên `S06_ir`.
- Bộ phát hiện cho kết quả khác nhau theo nguồn ảnh và độ sáng. Một nhãn hợp lệ về cấu trúc vẫn có thể thiếu đối tượng.
- Khi tự rà, mở cả hai khung SVIRO để định vị vùng cần xem, rồi chốt từng nhãn từ chi tiết thấy được trong chính khung đang gán. Với DMS, thử chỉnh độ sáng hiển thị trên CVAT và dùng quy tắc §4.5, §4.6.
- Trước lần export này, thuộc tính `glasses` ở `D04` đã được đổi từ `sun` sang `unknown` vì vùng kính trên NIR chưa đủ rõ để xác nhận loại kính.

## 3. Tổng hợp kết quả đối chiếu

- `disagreement_log.csv` ghi 30 dòng đối chiếu trên 22 khung: 5 trường hợp đã cập nhật ở V2, 5 trường hợp giữ nguyên nhãn CVAT và 20 trường hợp cần xác minh thêm.
- `S01_ir`: mô tả đối tượng còn mơ hồ giữa người và ghế trẻ em; cần hỏi coach theo GUIDE §3. `S06_ir`: phân loại ghế chưa thống nhất, cần đối chiếu tay xách và hướng nôi.
- DMS: box mặt có kết quả khớp ở 6/6 ảnh, box tay khớp 4/7 theo ngưỡng IoU 0,50. Riêng `D06`, độ lệch trung bình của 5 điểm mặt là 75,45 px; cần rà lại mũi, miệng và cờ che khuất.
- **Nguồn đối chiếu:** E3 và bảng đo DMS sử dụng mô hình chạy local (Qwen3-VL, YOLOE, MediaPipe), không phải lượt review độc lập của người khác. Chi tiết nguồn và từng trường hợp được lưu trong `disagreement_log.csv` và `prompts.md`.

## 4. Đối chiếu hai bản nháp đã lưu

Bảng dưới ghi 10 thay đổi thực tế trên 22 khung chung. SHA-256 của hai ZIP bắt đầu bằng `16b6a8ba2ca4` và `fbd50a09df1e` theo thứ tự trước và sau khi bổ sung nhãn.

- Tám box mới ở `S04` và `S06` dùng cùng mã ghế giữa hai nguồn ảnh. Độ chồng lấp RGB/IR là 0,847–1,000. Mức chồng lấp chỉ xác nhận vị trí nhất quán. Cần rà phân loại ghế và hướng nôi trực tiếp.
- `D06` có thêm box mặt và đủ 13 điểm. Điểm khóe ngoài mắt trái có y lớn hơn điểm tương ứng của mắt phải. Tám điểm mắt có cờ `occluded` và cần rà trực tiếp.
- Tất cả box và điểm mặt đều nằm trong ảnh. Kết quả kiểm cấu trúc của bản cập nhật là 0 lỗi, 0 cảnh báo.

| Khung | Nhãn | Trường | Trước | Sau | Nhận định | Quy tắc | Xử lý |
| --- | --- | --- | --- | --- | --- | --- | --- |
| D06 | face | box | Chưa có box mặt | Box 275 × 247 px, hai mắt unknown | Giữ box mặt nháp | §4.4-5, §4.5 | Rà trán, cằm và vùng mắt trên CVAT trước khi chốt biên hoặc trạng thái mắt. |
| D06 | face13 | count | 0 skeleton | 1 skeleton, 13 điểm, 8 điểm mắt có cờ occluded | Đạt kiểm tra số điểm và trục y, chờ rà cờ | §4.4-2, §4.6 | Mắt trái y=723,3 px, mắt phải y=632,2 px. Rà từng điểm bị che và đổi outside khi không định vị được. |
| S04_ir | infant_seat | box | Chưa có box | rear_right, facing=rear, IoU RGB/IR 0,847 | Giữ đề xuất, chờ xác nhận loại ghế | §4.4-3, §4.8 | Kiểm tra riêng tay xách, biên nôi và hướng đặt trên khung này. |
| S04_ir | child_seat | box | Chưa có box | rear_left, occupied=unknown, IoU RGB/IR 0,987 | Giữ đề xuất, chờ xác nhận biên ghế | §4.4-3, §4.8 | Rà biên giữa ghế dành cho trẻ em và ghế gốc. Kiểm tra lòng ghế trước khi cập nhật occupied. |
| S04_rgb | infant_seat | box | Chưa có box | rear_right, facing=rear, IoU RGB/IR 0,847 | Giữ đề xuất, chờ xác nhận loại ghế | §4.4-3, §4.8 | Kiểm tra riêng tay xách, biên nôi và hướng đặt trên khung này. |
| S04_rgb | child_seat | box | Chưa có box | rear_left, occupied=unknown, IoU RGB/IR 0,987 | Giữ đề xuất, chờ xác nhận biên ghế | §4.4-3, §4.8 | Rà biên giữa ghế dành cho trẻ em và ghế gốc. Kiểm tra lòng ghế trước khi cập nhật occupied. |
| S06_ir | infant_seat | box | Chưa có box | rear_right, facing=rear, IoU RGB/IR 1,000 | Giữ đề xuất, chờ xác nhận loại ghế | §4.4-3, §4.8 | Kiểm tra riêng tay xách, biên nôi và hướng đặt trên khung này. |
| S06_ir | child_seat | box | Chưa có box | rear_left, occupied=unknown, IoU RGB/IR 0,984 | Giữ đề xuất, chờ xác nhận biên ghế | §4.4-3, §4.8 | Rà biên giữa ghế dành cho trẻ em và ghế gốc. Kiểm tra lòng ghế trước khi cập nhật occupied. |
| S06_rgb | child_seat | box | Chưa có box | rear_left, occupied=unknown, IoU RGB/IR 0,984 | Giữ đề xuất, chờ xác nhận biên ghế | §4.4-3, §4.8 | Rà biên giữa ghế dành cho trẻ em và ghế gốc. Kiểm tra lòng ghế trước khi cập nhật occupied. |
| S06_rgb | infant_seat | box | Chưa có box | rear_right, facing=rear, IoU RGB/IR 1,000 | Giữ đề xuất, chờ xác nhận loại ghế | §4.4-3, §4.8 | Kiểm tra riêng tay xách, biên nôi và hướng đặt trên khung này. |

## 5. Các cập nhật ở bản V2

- `S03_rgb` và `S03_ir` có thêm `object` ở `rear_left`, tạm để `type=other` do chưa đủ dấu hiệu phân loại. Đồ vật ở `rear_right` của `S03_ir` đổi `type` từ `other` sang `bag` vì lượt rà ghi được tay xách và chi tiết mặt túi.
- `S07_rgb` và `S07_ir` có thêm `child_seat` ở `rear_middle`. `occupied` giữ `unknown` trong khi chờ nhìn rõ lòng ghế.
- `export_final_v2.zip` là export CVAT sau các sửa đổi trên. `check_final_v2.txt` xác nhận 22 khung, 0 lỗi, 0 cảnh báo về cấu trúc.

## 6. Phần mở rộng S09–S10

Chọn phương án (a) trong GUIDE §5. Bốn khung được gán trong task CVAT riêng `Day12-G07-extra`. Bảng dưới ghi phương án nhãn hiện có và các điểm cần xác minh khi có nhãn gốc.

| Cảnh | Ghế phải xe `rear_right` | Ghế giữa `rear_middle` | Ghế trái xe `rear_left` |
|---|---|---|---|
| `S09_rgb`, `S09_ir` | Chưa có box đối tượng | `child_seat` với `occupied=unknown` | Chưa có box đối tượng |
| `S10_rgb`, `S10_ir` | `person` và box `face` | Chưa có box đối tượng | `child_seat` với `occupied=unknown` |

### Các trường hợp cần xác minh

| Ca | Nhận định | Cách xử lý |
|---|---|---|
| `S09_rgb` · vị trí vùng ghế | Sai ánh xạ trong đề xuất định vị. Tâm box đề xuất ở khoảng 50,7% chiều ngang, thuộc vùng giữa. Bản mô tả RGB ghi vùng này là ghế phải. | Sửa thành `rear_middle` theo quy ước xe. Giữ cùng mã ở ảnh IR-sim, nơi vùng tương ứng cũng nằm giữa khung. |
| `S09` · phân biệt người và ghế | Nghi ngờ sai lớp trong đề xuất `person`. Hai lượt mô tả vùng giữa giống trẻ nhỏ, trong khi bộ lab được mô tả là không có trẻ em ngồi. Có thêm đề xuất vị trí vỏ ghế trong cùng vùng. | Giữ đề xuất `child_seat` với `occupied=unknown`. Đối chiếu hiện vật ở ghế giữa với nhãn gốc theo §3 và §4.8. |
| `S10` · loại ghế bên phải ảnh | Mơ hồ về lớp. Các lượt phân loại tự động cho kết quả khác nhau giữa ghế trẻ em, nôi và ghế gốc. | Ghi đề xuất `child_seat` ở `rear_left`. Giữ `occupied=unknown` và đối chiếu thành ghế, tay xách và hướng lắp. |
| `S10` · người bên trái ảnh | Định vị người và mặt nhất quán trên cả hai khung. | Gán `person` ở `rear_right` cùng box mặt. Giữ `seatbelt=unknown` cho vùng dây chưa xác nhận. |

Nhãn gốc SVIRO cho hai cảnh của bộ G07 và phản hồi của coach vẫn chờ đối chiếu. Các nhận định trong bảng phản ánh những trường hợp chưa thống nhất giữa các lượt định vị tự động.

### Câu hỏi cho coach

Ở `S09`, vùng giữa là ghế trẻ em hay nôi trong nhãn gốc? Ở `S10`, vùng bên phải ảnh có ghế riêng hay chỉ là phần ghế nguyên bản? Xác nhận hai vị trí này sẽ phân biệt ca sai loại với ca mơ hồ thật.

## 7. Phép đo đối chiếu DMS

Nguồn của bảng là MediaPipe chạy local. IoU đo mức chồng lấp giữa hai box. Độ lệch điểm mặt tính trên các điểm hiện trong bản CVAT.

| Khung | Nhãn | Số CVAT | Số mô hình | Số khớp | IoU từ 0,50 | Điểm đo | Lệch TB (px) | Xử lý |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| D01 | face | 1 | 1 | 1 | 0.746 | — | — | Giữ box đã khớp. Rà biên trên ảnh. |
| D01 | hand | 1 | 1 | 1 | 0.858 | — | — | Giữ box đã khớp. Rà biên trên ảnh. |
| D01 | face13 | 1 | — | — | — | 5 | 9.82 | Giữ face13 trong bản CVAT và rà cờ của từng điểm. |
| D02 | face | 1 | 1 | 1 | 0.720 | — | — | Giữ box đã khớp. Rà biên trên ảnh. |
| D02 | hand | 0 | 0 | 0 | — | — | — | Hai lượt không có box cần đối chiếu. |
| D02 | face13 | 1 | — | — | — | 5 | 10.27 | Giữ face13 trong bản CVAT và rà cờ của từng điểm. |
| D03 | face | 1 | 1 | 1 | 0.658 | — | — | Giữ box đã khớp. Rà biên trên ảnh. |
| D03 | hand | 2 | 1 | 1 | 0.708 | — | — | Giữ các box CVAT để kiểm tra lại, gồm các box chưa có đề xuất khớp. |
| D03 | face13 | 1 | — | — | — | 13 | 7.97 | Giữ face13 trong bản CVAT và rà cờ của từng điểm. |
| D04 | face | 1 | 1 | 1 | 0.664 | — | — | Giữ box đã khớp. Rà biên trên ảnh. |
| D04 | hand | 1 | 0 | 0 | — | — | — | Giữ các box CVAT để kiểm tra lại, gồm các box chưa có đề xuất khớp. |
| D04 | face13 | 1 | — | — | — | 5 | 4.96 | Giữ face13 trong bản CVAT và rà cờ của từng điểm. |
| D05 | face | 1 | 1 | 1 | 0.775 | — | — | Giữ box đã khớp. Rà biên trên ảnh. |
| D05 | hand | 2 | 1 | 1 | 0.752 | — | — | Giữ các box CVAT để kiểm tra lại, gồm các box chưa có đề xuất khớp. |
| D05 | face13 | 1 | — | — | — | 9 | 11.92 | Giữ face13 trong bản CVAT và rà cờ của từng điểm. |
| D06 | face | 1 | 1 | 1 | 0.510 | — | — | Giữ box đã khớp. Rà biên trên ảnh. |
| D06 | hand | 1 | 1 | 1 | 0.789 | — | — | Giữ box đã khớp. Rà biên trên ảnh. |
| D06 | face13 | 1 | — | — | — | 5 | 75.45 | Lệch vị trí đáng kể giữa hai bộ phát hiện. Rà lại mũi và miệng trực tiếp trên ảnh. |

## Phụ lục A. Dữ liệu XML dùng để đối chiếu

XML lưu đủ 22 khung của bản nhãn đối chiếu tự động. Phần tử `meta` giữ nguyên nguồn tạo và SHA-256 của `export_final.zip` phục vụ truy vết.

<details>
<summary>XML đối chiếu</summary>

```xml
<annotations>
  <version>1.1</version>
  <image id="0" name="S01_rgb.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">rgb</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="person" source="auto" occluded="0" xtl="355.20" ytl="224.00" xbr="547.20" ybr="467.20">
      <attribute name="seat">rear_left</attribute>
    </box>
  </image>
  <image id="1" name="S01_ir.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">ir</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="person" source="auto" occluded="0" xtl="355.20" ytl="229.12" xbr="574.08" ybr="525.44">
      <attribute name="seat">rear_left</attribute>
    </box>
  </image>
  <image id="2" name="S02_rgb.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">rgb</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="person" source="auto" occluded="0" xtl="564.48" ytl="96.00" xbr="878.40" ybr="638.72">
      <attribute name="seat">rear_left</attribute>
    </box>
    <box label="child_seat" source="auto" occluded="0" xtl="0.00" ytl="320.00" xbr="388.80" ybr="638.72">
      <attribute name="seat">rear_right</attribute>
    </box>
  </image>
  <image id="3" name="S02_ir.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">ir</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="person" source="auto" occluded="0" xtl="561.60" ytl="94.72" xbr="875.52" ybr="638.72">
      <attribute name="seat">rear_left</attribute>
    </box>
    <box label="child_seat" source="auto" occluded="0" xtl="0.00" ytl="316.16" xbr="390.72" ybr="638.72">
      <attribute name="seat">rear_right</attribute>
    </box>
  </image>
  <image id="4" name="S03_rgb.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">rgb</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="infant_seat" source="auto" occluded="0" xtl="350.40" ytl="316.16" xbr="628.80" ybr="617.60">
      <attribute name="seat">rear_middle</attribute>
    </box>
    <box label="object" source="auto" occluded="0" xtl="192.00" ytl="320.00" xbr="372.48" ybr="542.72">
      <attribute name="seat">rear_right</attribute>
      <attribute name="type">bag</attribute>
    </box>
  </image>
  <image id="5" name="S03_ir.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">ir</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="infant_seat" source="auto" occluded="0" xtl="343.68" ytl="316.16" xbr="631.68" ybr="615.68">
      <attribute name="seat">rear_middle</attribute>
    </box>
    <box label="object" source="auto" occluded="0" xtl="190.08" ytl="337.92" xbr="366.72" ybr="542.72">
      <attribute name="seat">rear_right</attribute>
      <attribute name="type">bag</attribute>
    </box>
    <box label="object" source="auto" occluded="0" xtl="602.88" ytl="400.00" xbr="648.00" ybr="448.00">
      <attribute name="seat">rear_left</attribute>
      <attribute name="type">other</attribute>
    </box>
  </image>
  <image id="6" name="S04_rgb.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">rgb</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="child_seat" source="auto" occluded="0" xtl="561.60" ytl="161.28" xbr="930.24" ybr="638.72">
      <attribute name="seat">rear_left</attribute>
    </box>
    <box label="infant_seat" source="auto" occluded="0" xtl="0.00" ytl="320.00" xbr="382.08" ybr="638.72">
      <attribute name="seat">rear_right</attribute>
    </box>
  </image>
  <image id="7" name="S04_ir.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">ir</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="child_seat" source="auto" occluded="0" xtl="561.60" ytl="112.00" xbr="929.28" ybr="639.36">
      <attribute name="seat">rear_left</attribute>
    </box>
    <box label="infant_seat" source="auto" occluded="0" xtl="0.00" ytl="112.00" xbr="382.08" ybr="639.36">
      <attribute name="seat">rear_right</attribute>
    </box>
  </image>
  <image id="8" name="S05_rgb.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">rgb</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="infant_seat" source="auto" occluded="0" xtl="0.00" ytl="316.16" xbr="385.92" ybr="624.00">
      <attribute name="seat">rear_right</attribute>
    </box>
  </image>
  <image id="9" name="S05_ir.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">ir</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="infant_seat" source="auto" occluded="0" xtl="0.00" ytl="316.16" xbr="385.92" ybr="624.00">
      <attribute name="seat">rear_right</attribute>
    </box>
  </image>
  <image id="10" name="S06_rgb.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">rgb</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="child_seat" source="auto" occluded="0" xtl="0.00" ytl="312.32" xbr="384.00" ybr="622.08">
      <attribute name="seat">rear_right</attribute>
    </box>
    <box label="child_seat" source="auto" occluded="0" xtl="564.48" ytl="158.72" xbr="931.20" ybr="638.72">
      <attribute name="seat">rear_left</attribute>
    </box>
  </image>
  <image id="11" name="S06_ir.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">ir</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="child_seat" source="auto" occluded="0" xtl="0.00" ytl="312.32" xbr="384.00" ybr="622.08">
      <attribute name="seat">rear_right</attribute>
    </box>
    <box label="child_seat" source="auto" occluded="0" xtl="561.60" ytl="163.20" xbr="931.20" ybr="639.36">
      <attribute name="seat">rear_left</attribute>
    </box>
  </image>
  <image id="12" name="S07_rgb.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">rgb</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="child_seat" source="auto" occluded="0" xtl="362.88" ytl="201.60" xbr="595.20" ybr="622.08">
      <attribute name="seat">rear_middle</attribute>
    </box>
    <box label="object" source="auto" occluded="0" xtl="211.20" ytl="353.28" xbr="382.08" ybr="590.08">
      <attribute name="seat">rear_right</attribute>
      <attribute name="type">box</attribute>
    </box>
  </image>
  <image id="13" name="S07_ir.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">ir</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="child_seat" source="auto" occluded="0" xtl="362.88" ytl="142.08" xbr="595.20" ybr="622.08">
      <attribute name="seat">rear_middle</attribute>
    </box>
  </image>
  <image id="14" name="S08_rgb.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">rgb</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="person" source="auto" occluded="0" xtl="529.92" ytl="80.00" xbr="960.00" ybr="639.36">
      <attribute name="seat">rear_left</attribute>
    </box>
    <box label="child_seat" source="auto" occluded="0" xtl="115.20" ytl="128.00" xbr="403.20" ybr="624.00">
      <attribute name="seat">rear_right</attribute>
    </box>
  </image>
  <image id="15" name="S08_ir.png" width="960" height="640">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">ir</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="person" source="auto" occluded="0" xtl="529.92" ytl="80.00" xbr="960.00" ybr="639.36">
      <attribute name="seat">rear_left</attribute>
    </box>
    <box label="person" source="auto" occluded="0" xtl="94.08" ytl="198.40" xbr="391.68" ybr="536.32">
      <attribute name="seat">rear_right</attribute>
    </box>
  </image>
  <image id="16" name="D01.png" width="1280" height="1024">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">ir</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="person" source="auto" occluded="0" xtl="217.60" ytl="346.11" xbr="1213.44" ybr="865.28">
      <attribute name="seat">driver</attribute>
    </box>
    <box label="face" source="auto" occluded="0" xtl="842.54" ytl="366.29" xbr="1117.97" ybr="630.30" />
    <box label="hand" source="auto" occluded="0" xtl="415.02" ytl="506.17" xbr="536.32" ybr="706.88">
      <attribute name="side">left</attribute>
    </box>
  </image>
  <image id="17" name="D02.png" width="1280" height="1024">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">ir</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="person" source="auto" occluded="0" xtl="1.28" ytl="227.33" xbr="1255.68" ybr="901.12">
      <attribute name="seat">driver</attribute>
    </box>
    <box label="face" source="auto" occluded="0" xtl="901.99" ytl="299.07" xbr="1204.45" ybr="583.23" />
  </image>
  <image id="18" name="D03.png" width="1280" height="1024">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">ir</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="person" source="auto" occluded="0" xtl="192.00" ytl="378.88" xbr="1231.36" ybr="909.31">
      <attribute name="seat">driver</attribute>
    </box>
    <box label="face" source="auto" occluded="0" xtl="961.03" ytl="436.99" xbr="1180.20" ybr="663.38" />
    <box label="hand" source="auto" occluded="0" xtl="409.34" ytl="435.35" xbr="530.43" ybr="528.41">
      <attribute name="side">right</attribute>
    </box>
  </image>
  <image id="19" name="D04.png" width="1280" height="1024">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">ir</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="person" source="auto" occluded="0" xtl="192.00" ytl="366.59" xbr="1228.80" ybr="885.76">
      <attribute name="seat">driver</attribute>
    </box>
    <box label="face" source="auto" occluded="0" xtl="884.70" ytl="434.26" xbr="1144.96" ybr="694.01" />
  </image>
  <image id="20" name="D05.png" width="1280" height="1024">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">ir</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="person" source="auto" occluded="0" xtl="224.00" ytl="360.45" xbr="1228.80" ybr="844.80">
      <attribute name="seat">driver</attribute>
    </box>
    <box label="face" source="auto" occluded="0" xtl="858.83" ytl="376.46" xbr="1132.40" ybr="648.33" />
    <box label="hand" source="auto" occluded="0" xtl="347.41" ytl="541.43" xbr="449.66" ybr="666.62">
      <attribute name="side">left</attribute>
    </box>
  </image>
  <image id="21" name="D06.png" width="1280" height="1024">
    <tag label="frame_meta" source="auto">
      <attribute name="modality">ir</attribute>
      <attribute name="camera_blocked">no</attribute>
      <attribute name="light">unknown</attribute>
    </tag>
    <box label="person" source="auto" occluded="0" xtl="211.20" ytl="421.89" xbr="1228.80" ybr="974.85">
      <attribute name="seat">driver</attribute>
    </box>
    <box label="face" source="auto" occluded="0" xtl="836.75" ytl="496.56" xbr="1170.14" ybr="798.50" />
    <box label="hand" source="auto" occluded="0" xtl="337.28" ytl="838.74" xbr="525.72" ybr="962.85">
      <attribute name="side">left</attribute>
    </box>
  </image>
  <meta>
    <review_source>Qwen3-VL and MediaPipe local AI review proposals</review_source>
    <purpose>Comparison reference for cabin_check diff. Source and limitations are recorded in prompts.md.</purpose>
    <base_cvat_sha256>fbd50a09df1e25e0117f1d7571e796a192b40cb43b1c3c98bc4db5689e9cb068</base_cvat_sha256>
  </meta>
</annotations>
```

</details>
