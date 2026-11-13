# Báo cáo cá nhân — LuongDucManh-2A202603155

Đổi tên file theo quy tắc `MSSV_HoTen.md`. Ví dụ: `20241234_NguyenVanAn.md`.

Dữ liệu: 12 ảnh người COCO val2017 (Lin et al., 2014) và 8 ảnh chân dung Wikimedia Commons, chỉ dùng trong lớp.

## 0. Thông tin học viên

- Họ tên: Lương Đức Mạnh
- MSSV: 2A202603155
- Email: 369ldmanh@gmail.com
- Link repository: https://github.com/ldmanh610/K5-L2A-DAY04-LuongDucManh-2A202603155-KeypointPose
- Phiên bản CVAT (Help → About): 2.82.1
- Máy: hệ điều hành Windows 11 · RAM 16 GB · chạy CVAT chế độ đầy đủ
- Bộ ảnh (`src/exports/assignment.json` do `lab.py setup` tạo): có
- Giờ bắt đầu: (18h30)
- Giờ kết thúc: (11h58)

## 1. Kiểm tra label (CP0)

| Label | Kiểu | Số điểm | Tên điểm đúng bảng của lab? | Đúng thứ tự? | Số cạnh |
|---|---|---:|---|---|---:|
| `person` | Skeleton | 17 | Có | Có | 19 |
| `face` | Skeleton | 13 | Có | Có | 11 |

Mẫu `person` dựng theo người quay mặt về camera (các điểm `left_*` nằm bên phải khung mẫu)? Có

Cách đổi cờ `occluded` / `outside` cho **một điểm** trên phiên bản CVAT này (một câu):

Nhấp chuột phải trực tiếp vào điểm keypoint cần chỉnh sửa trên khung hình (hoặc mở danh sách các điểm của skeleton ở bảng Objects bên phải) và chọn biểu tượng bật/tắt cờ `occluded` hoặc `outside` chỉ dành riêng cho điểm đó.

## 2. Số lượng đã gán và thời gian (CP1, CP2)

| Nhóm ảnh | Số ảnh | Số skeleton đã gán | Số người dưới 60 px (không gán) | Số poster / gương / màn hình bỏ qua | Thời gian (phút) |
|---|---:|---:|---:|---:|---:|
| `easy` | 5 | 5 | 1 | 0 | 25 |
| `occluded` | 3 | 6 | 0 | 0 | 25 |
| `back` | 2 | 4 | 0 | 0 | 18 |
| `small` | 1 | 1 | 5 | 0 | 12 |
| `crowd` | 1 | 2 | 6 | 0 | 15 |
| `faces` | 8 | 8 | — | 0 | 25 |

- Số ảnh người đã xong ở mốc phút 105: 9 / 10
- Thời gian thực tế (phút): CP1 95 · CP2 25 · CP3 20
- Ảnh tốn thời gian nhất là ảnh nào, vì sao?

Ảnh `000000130816.jpg` (nhóm `crowd`) và `000000253584.jpg` (nhóm `small`) tốn nhiều thời gian nhất vì có nhiều người nhỏ ở hậu cảnh cần đo kích thước pixel để lọc bỏ các đối tượng dưới 60 px, đồng thời xử lý chồng lấn giữa hai cầu thủ đứng trước - sau và đặt cờ che khuất (`occluded`/`outside`) cho các khớp bị lan can và thân người che mất.

## 3. Bảng ca khó (CP1, CP2)

*(Tự điền các điểm bạn thực sự phân vân trong quá trình gán nhãn trên CVAT)*

| Ảnh | Người / mặt | Điểm | Cờ đã chọn | Phân vân giữa | Lý do chọn |
|---|---|---|---:|---|---|
| `000000130816.jpg` | Người #1 (cầu thủ trước) | `left_ankle`, `right_ankle` | `v = 1` | `v = 1` và `v = 0` | Bị thanh lan can màu xanh che ngang cổ chân nhưng vẫn nhìn rõ hướng ống quần nên ước lượng chính xác được vị trí khớp. |
| `000000130816.jpg` | Người #2 (cầu thủ sau) | `left_knee`, `right_knee`, `left_ankle`, `right_ankle` | `v = 0` | `v = 1` và `v = 0` | Toàn bộ phần thân dưới bị cầu thủ phía trước che khuất hoàn toàn, không có cơ sở hình học để đoán chính xác tư thế chân. |
| (Ảnh kính râm) | Mặt #1 | 4 điểm vùng mắt | `v = 1` | `v = 1` và `v = 2` | Mắt bị che sau tròng kính râm tối màu nhưng vẫn ước lượng được vị trí theo gọng kính và cấu trúc hốc mắt. |

## 4. Bảng tự review (CP3)

*(Tự điền tối thiểu 8 dòng gồm 5 ảnh người + 3 ảnh mặt từ quá trình review ngược của bạn)*

| Ảnh | Người / mặt | Điểm | Mã lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|---|
| `000000130816.jpg` | Người #1, #2 | Toàn bộ cặp `left_*` / `right_*` | `lr_swap` | Gán nhầm trái/phải theo góc nhìn người xem thay vì hướng mặt của cầu thủ | Đã sửa |
| `000000130816.jpg` | Người #3–#8 | Toàn bộ skeleton | `extra` | Gán nhầm các khán giả ở xa trên khán đài có chiều cao dưới 60 px | Đã sửa |
| `000000253584.jpg` | Người #2–#6 | Toàn bộ skeleton | `extra` | Xóa các đối tượng người nhỏ ở hậu cảnh có chiều cao 16–46 px (< 60 px) | Đã sửa |
| `000000461002.jpg` | Người #1 | `left_shoulder`, `right_shoulder` | `ok` | Người quay lưng, đã đảo đúng cụm `left_*` sang bên trái khung hình | Không cần sửa |
| `000000307757.jpg` | Người #1 | Toàn bộ skeleton | `ok` | Đúng thứ tự khớp và cờ hiển thị | Không cần sửa |
| `face_342273.jpg` | Mặt #2 | Toàn bộ skeleton | `extra` | Dư 1 skeleton thứ hai trên ảnh khuôn mặt | Đã sửa |
| `face_10579.jpg` | Mặt #1 | `left_eye_upper`, `left_eye_lower` | `position` | Tinh chỉnh lại điểm nằm sát mép mí mắt thay vì nằm trên lông mi | Đã sửa |
| `face_85624.jpg` | Mặt #1 | Toàn bộ skeleton | `ok` | Đúng mắt trái/phải và khóe miệng | Không cần sửa |

Tổng hợp số lỗi theo mã:

| `lr_swap` | `vis_flag` | `position` | `missing` | `extra` | `ok` |
|---:|---:|---:|---:|---:|---:|
| 1 | 0 | 1 | 0 | 3 | 3 |

Điểm còn phân vân giữa `v = 1` và `v = 0` (ghi cả hai lựa chọn và lý do chọn):

Ở ảnh `000000130816.jpg` (cầu thủ đứng phía sau), phần hông (`left_hip`, `right_hip`) nằm sát ranh giới bị che bởi vai và lưng cầu thủ đứng trước: phân vân giữa `v = 1` (ước lượng theo trục vai thẳng xuống) và `v = 0` (bị che khuất hoàn toàn). Em chọn `v = 0` cho các khớp chân bên dưới vì tư thế khuỵu gối không thể quan sát được, tránh tạo nhiễu tọa độ đoán bừa khi huấn luyện model.

## 5. Kiểm file xuất và kết quả tự kiểm (CP4)

Tự kiểm tay trong file JSON:

| File | Người / mặt kiểm (ảnh, thứ tự) | Số phần tử `keypoints` | Điểm `outside` ra `v = 0`, `occluded` ra `v = 1`? | `num_keypoints` khớp? |
|---|---|---:|---|---|
| `LuongDucManh_persons.zip` | `000000130816.jpg`, #1 | 51 | Có | Có |
| `LuongDucManh_persons.zip` | `000000130816.jpg`, #2 | 51 | Có | Có |
| `LuongDucManh_faces.zip` | `face_342273.jpg`, #1 | 39 | Có | Có |

Sau khi xuất, CVAT ghi `occluded` thành `v = 1` và `outside` thành `v = 0`. Tọa độ của điểm `outside` trong file là `0, 0` hay tọa độ cũ? Tọa độ cũ

Hai job đã chuyển sang completed trên CVAT? Có

Dán phần **TÓM TẮT** in ra từ `src/check_submission.py`:

```text
- File person: LuongDucManh_persons.zip
  Số ảnh: 12 · số skeleton `person`: 18 · mỗi annotation 51 số
  Skeleton theo ảnh: 000000109010=2, 000000130816=2, 000000220045=2, 000000225138=1, 000000241837=1, 000000253584=1, 000000292999=1, 000000294379=1, 000000302102=2, 000000307757=2, 000000455536=1, 000000461002=2
  Cờ v: v=2 263 (86%) · v=1 17 (6%) · v=0 26 (8%)
  num_keypoints trung bình: 15.6
- File face: LuongDucManh_faces.zip
  Số ảnh: 8 · số skeleton `face`: 8 · mỗi annotation 39 số
  Skeleton theo ảnh: face_10579=1, face_141135=1, face_296767=1, face_342273=1, face_356144=1, face_416730=1, face_79367=1, face_85624=1
  Cờ v: v=2 96 (92%) · v=1 8 (8%) · v=0 0 (0%)
  num_keypoints trung bình: 13.0
- Ghi chú: 26 điểm v = 0 vẫn mang tọa độ cũ (CVAT 2.x xuất như vậy; chuẩn COCO ghi 0, 0). Không phải lỗi: điểm v = 0 bị bỏ qua khi huấn luyện và khi chấm.
- Số [LỖI]: 0 · số [CẢNH BÁO]: 2
Kết quả: ĐẠT (đọc lại từng cảnh báo ở trên)
```

Các `[CẢNH BÁO]` còn giữ nguyên và lý do:
```
[CẢNH BÁO] 000000292999.jpg #1: cặp elbow nằm ngược phía... và [CẢNH BÁO] 000000294379.jpg #1: cặp ankle nằm ngược phía...: Giữ nguyên vì nhân vật trong ảnh thực tế đang ở tư thế vắt tay / bắt chéo chân tự nhiên, không phải lỗi đảo nhãn trái/phải (lr_swap).
```
...

## 6. Ảnh chụp màn hình

1. Label `person` trong label constructor (đủ 17 điểm có tên):

   ![Label person](images/1_label_person.png)

2. Một người quay lưng đã gán, cờ đã đặt:

   ![Người quay lưng](images/2_nguoi_quay_lung.png)

3. Một khuôn mặt đã gán 13 điểm, phóng to vùng mắt:

   ![Khuôn mặt phóng to](images/3_mat_phong_to.png)

4. Một đoạn JSON xuất của một người:

   ![JSON export](images/4_json_export.png)

Nguồn ảnh: COCO val2017 / Wikimedia Commons.

## 7. Tự đánh giá

1. Lỗi bạn mắc nhiều nhất hôm nay, và cách bạn sẽ tránh ở dự án thật:
```
Lỗi mắc nhiều nhất là gán thừa các đối tượng nhỏ dưới ngưỡng kích thước quy định (< 60 px) ở hậu cảnh và nhầm lẫn hướng trái/phải theo góc nhìn của người gán nhãn thay vì trục giải phẫu của nhân vật. Ở dự án thực tế, cần bật hiển thị kích thước bounding box ngay từ đầu để lọc ngưỡng pixel và luôn xác định hướng mặt nhân vật trước khi kéo điểm.
```
   ...

2. Một quy tắc bạn đề xuất thêm vào guideline (kèm ảnh ví dụ nếu có):
```
Bổ sung quy tắc kiểm tra nhanh bằng lưới bounding box tối thiểu (ví dụ khung tham chiếu 60×60 px) trên CVAT trước khi đặt skeleton trong các ảnh đám đông (crowd), tránh mất thời gian gán nhãn cho các đối tượng ở quá xa rồi phải xóa bỏ.
```
   ...

3. Nhãn của bạn đi vào model ở đâu? Chọn một lỗi bạn đã sửa và nói nó sẽ làm model học sai thế nào (heatmap, trọng số trong loss, hay bước lật ảnh):
```
Nhãn keypoint được chuyển thành các bản đồ nhiệt (heatmap) 2D làm ground truth để tính hàm mất mát (loss) với các điểm có cờ v > 0. Nếu để sót lỗi lr_swap (đảo trái/phải), khi huấn luyện áp dụng kỹ thuật tăng cường dữ liệu lật ngang ảnh (horizontal flip) và hoán đổi cặp kênh trái/phải, nhãn sẽ bị gán sai hoàn toàn sang bên đối diện, làm sụp đổ chỉ số OKS và khiến model dự đoán ngược khớp.
```
...

## 8. Khó khăn và lỗi kỹ thuật

```
Trong quá trình đẩy bài lên GitHub gặp lỗi xác thực 403 Permission denied do Windows Credential Manager lưu thông tin đăng nhập của tài khoản GitHub cũ. Đã khắc phục bằng cách sử dụng Personal Access Token (PAT) trực tiếp trong lệnh git push để đẩy bài lên đúng repository cá nhân
```

...

## Bonus (nếu làm)

### B1 — 3 ảnh người khó thêm

| Ảnh | Số skeleton | Số người dưới 60 px | Ca khó nhất |
|---|---:|---:|---|
| | | | |
| | | | |
| | | | |

Kết quả `src/check_submission.py` cho `HoVaTen_bonus_persons.zip`:

```text
(dán ở đây)
```

### B2 — 5 ảnh khuôn mặt đen trắng / hồng ngoại

Ảnh không màu khó hơn ở điểm nào? Kính râm trên ảnh hồng ngoại có còn là `v = 1` không?

...

### B3 — Model so với nhãn của tôi (không tính điểm)

- Demo đã dùng:
- Ảnh đã thử:

1. Model có trả điểm cho khớp bị che, hay khớp ngoài ảnh không? Confidence ở các điểm đó thế nào?

   ...

2. Với người quay lưng, model có đổi trái/phải không?

   ...

3. Bộ điểm của model có giống 17 điểm COCO không?

   ...

## 9. Tài liệu tham khảo và công cụ AI đã dùng

...
