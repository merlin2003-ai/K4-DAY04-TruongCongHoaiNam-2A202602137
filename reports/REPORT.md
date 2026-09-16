# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Trương Công Hoài Nam   Nhóm: 201   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20|
| Số skeleton | 29|
| v=2 / v=1 / v=0 | 357 / 109 / 27|
| Thời gian trung bình mỗi ảnh | |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear
2. right_ear
3. left_wrist

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.
   Đúng, vì không phải ai cũng nhìn thẳng để hiện rõ tai. Mọi người luôn có xu hướng nghiêng người, tai thì ở 2 bên đầu nên dễ bị ẩn hơn
<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.931| |
| OKS@0.50 | 1.000| 1.000|
| OKS@0.75 | 1.000| 1.000|
| Lỗi `dao_trai_phai` | 0| 0|
| Lỗi `nham_nguoi` | 2| 0|
| Lỗi `xoa_khop_bi_che` | 0| 0|

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):
<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- train_01.jpg người #2: right_wrist, đã kéo phần tay người người này xa ra khỏi tay người kia để tránh bị nhầm
- train_04.jpg người #1: left_wrist, đã kéo phần tay người người này xa ra khỏi tay người kia để tránh bị nhầm
-

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->
- Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.
## 3. Kiểm chéo

Bạn cùng nhóm: Ngô Văn Hưng

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| left_hip| 21%| 38%| 17| mỗi người đánh giá phần hông ở vị trí khác|
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

- kiểm chứng lại với file gold để đưa ra quyết định

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450| 0.8450| 0.0000|
| pose_mAP50-95 | 0.6853| 0.6908| 0.0055|
| pose_precision | 0.9734| 0.9792| 0.0058|
| pose_recall | 0.8462| 0.8462| 0.0000|
| box_mAP50-95 | 0.8119| 0.8041| -0.0078|

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

   pose_mAP50-95 tăng 0.0055

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

   Chênh lệch:0.8041 − 0.6908 = 0.1133. Bounding box chỉ cần xác định vùng chứa người tương đối chính xác. Pose keypoints yêu cầu xác định vị trí từng khớp cơ thể

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   Ảnh test_05, model đoán lệch nhẹ phần chân trái

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

   Ảnh train_06 có OKS=0.66 thấp nhất

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.
 
   Ở ảnh train_04, người thứ 1, phần right_hip ở gần sát mép ảnh, do trang phục rộng nên khó xác định được phần right_hip đó đã rời khỏi khung chưa
<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
