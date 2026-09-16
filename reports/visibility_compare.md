# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 27 skeleton, trung bình 16.0 khớp có v > 0 mỗi người
- Tổng: v=2 302 | v=1 130 | v=0 27

So sánh với `..\ban_cung_nhom\dataset\labels\train` (0 skeleton).
Cột **lệch** là hiệu số phần trăm v=1 - chỗ nào lệch nhiều nhất là chỗ guideline chưa nói rõ.

| # | Khớp | %v=1 (bạn) | %v=1 (đối chiếu) | lệch |
| ---: | --- | ---: | ---: | ---: |
| 3 | left_ear | 56% | 0% | 56 |
| 4 | right_ear | 41% | 0% | 41 |
| 1 | left_eye | 37% | 0% | 37 |
| 9 | left_wrist | 37% | 0% | 37 |
| 10 | right_wrist | 37% | 0% | 37 |
| 11 | left_hip | 37% | 0% | 37 |
| 12 | right_hip | 37% | 0% | 37 |
| 2 | right_eye | 30% | 0% | 30 |
| 13 | left_knee | 26% | 0% | 26 |
| 14 | right_knee | 26% | 0% | 26 |
| 16 | right_ankle | 26% | 0% | 26 |
| 0 | nose | 22% | 0% | 22 |
| 15 | left_ankle | 22% | 0% | 22 |
| 7 | left_elbow | 19% | 0% | 19 |
| 8 | right_elbow | 19% | 0% | 19 |
| 5 | left_shoulder | 7% | 0% | 7 |
| 6 | right_shoulder | 4% | 0% | 4 |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
