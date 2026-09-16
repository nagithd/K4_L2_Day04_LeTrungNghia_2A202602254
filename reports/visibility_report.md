# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 27 skeleton, trung bình 16.0 khớp có v > 0 mỗi người
- Tổng: v=2 302 | v=1 130 | v=0 27

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 6 | 0 | 22% |
| 1 | left_eye | 17 | 10 | 0 | 37% |
| 2 | right_eye | 19 | 8 | 0 | 30% |
| 3 | left_ear | 12 | 15 | 0 | 56% |
| 4 | right_ear | 16 | 11 | 0 | 41% |
| 5 | left_shoulder | 25 | 2 | 0 | 7% |
| 6 | right_shoulder | 26 | 1 | 0 | 4% |
| 7 | left_elbow | 22 | 5 | 0 | 19% |
| 8 | right_elbow | 22 | 5 | 0 | 19% |
| 9 | left_wrist | 17 | 10 | 0 | 37% |
| 10 | right_wrist | 16 | 10 | 1 | 37% |
| 11 | left_hip | 17 | 10 | 0 | 37% |
| 12 | right_hip | 17 | 10 | 0 | 37% |
| 13 | left_knee | 15 | 7 | 5 | 26% |
| 14 | right_knee | 15 | 7 | 5 | 26% |
| 15 | left_ankle | 13 | 6 | 8 | 22% |
| 16 | right_ankle | 12 | 7 | 8 | 26% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
