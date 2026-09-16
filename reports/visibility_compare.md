# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 16.07 khớp có v > 0 mỗi người
- Tổng: v=2 357 | v=1 109 | v=0 27

So sánh với `gold\labels\train` (29 skeleton).
Cột **lệch** là hiệu số phần trăm v=1 - chỗ nào lệch nhiều nhất là chỗ guideline chưa nói rõ.

| # | Khớp | %v=1 (bạn) | %v=1 (đối chiếu) | lệch |
| ---: | --- | ---: | ---: | ---: |
| 3 | left_ear | 55% | 3% | 52 |
| 4 | right_ear | 41% | 14% | 28 |
| 9 | left_wrist | 31% | 7% | 24 |
| 2 | right_eye | 24% | 0% | 24 |
| 1 | left_eye | 24% | 3% | 21 |
| 0 | nose | 21% | 3% | 17 |
| 7 | left_elbow | 17% | 7% | 10 |
| 8 | right_elbow | 14% | 3% | 10 |
| 16 | right_ankle | 14% | 24% | 10 |
| 11 | left_hip | 21% | 28% | 7 |
| 14 | right_knee | 17% | 14% | 3 |
| 15 | left_ankle | 14% | 17% | 3 |
| 5 | left_shoulder | 10% | 7% | 3 |
| 10 | right_wrist | 24% | 21% | 3 |
| 13 | left_knee | 24% | 21% | 3 |
| 6 | right_shoulder | 3% | 3% | 0 |
| 12 | right_hip | 21% | 21% | 0 |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
