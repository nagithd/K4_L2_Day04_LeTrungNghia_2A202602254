# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Lê Trung Nghĩa   Nhóm: SOLO   Ngày: 16/09/2026

## 1. Nhãn của tôi

| Chỉ số                         |                                   Giá trị |
| -------------------------------- | ------------------------------------------: |
| Số ảnh đã gán               |                                          20 |
| Số skeleton                     |                                          27 |
| v=2 / v=1 / v=0                  |                              302 / 130 / 27 |
| Thời gian trung bình mỗi ảnh | Không có log thời gian gán trong output |

Ba khớp có `%v=1` cao nhất:

1. `left_ear`: 56% (15/27).
2. `right_ear`: 41% (11/27).
3. Đồng hạng 37%: `left_eye`, `left_wrist`, `right_wrist`, `left_hip`, `right_hip`.

Các khớp tai xuất hiện nhiều nhất ở trạng thái `v=1` vì ảnh có tư thế quay nghiêng, mũ bảo hiểm, tóc hoặc phần đầu quay khỏi camera. Đây là dấu hiệu thường bị che, nhưng vị trí giải phẫu vẫn có thể suy ra từ mắt, đường viền đầu và cổ. Cổ tay và hông cũng đạt 37% `v=1`, chủ yếu khi bị tay lái, xe máy, bàn hoặc thân người che; chúng khó xác định chính xác hơn tai vì không phải lúc nào cũng thấy được phần cơ thể kề bên.

## 2. Chấm với gold

| Chỉ số                |                        Trước rework | Sau rework |
| ----------------------- | ------------------------------------: | ---------: |
| OKS trung bình         | Sửa theo cảnh báo trước khi eval |     0.9562 |
| OKS@0.50                | Sửa theo cảnh báo trước khi eval |      0.931 |
| OKS@0.75                | Sửa theo cảnh báo trước khi eval |      0.931 |
| Lỗi`dao_trai_phai`   |                                     0 |          0 |
| Lỗi`nham_nguoi`      |                                     0 |          0 |
| Lỗi`xoa_khop_bi_che` |                                     0 |          0 |

**Tôi đã sửa gì giữa hai lần chạy**

- Tôi đã thêm và sửa các key point vào các ảnh train_01, train_04, train_10, train_11, train_16 theo cảnh báo sau khi chạy check_pose_labels
- Lần đánh giá hiện tại phát hiện thiếu 2 người ở `train_13.jpg`; đây là việc cần bổ sung skeleton khi quay lại CVAT. Theo như GUIDELINE_MINI tôi đã bỏ qua 2 người bị mờ phía sau và tập trung vào chủ thể chính của bức ảnh
- `train_06.jpg` có một lỗi `lech_nhe` ở `right_ear` (lệch 20 px), cần đặt lại điểm tai phải sát vị trí giải phẫu hơn.

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh theo `eval_vs_gold.json`. Tuy vậy, vẫn cần kiểm hình dáng skeleton vì công cụ kiểm định từng cảnh báo khả năng trái/phải ở các nhãn ban đầu.

## 3. Kiểm chéo

Bạn cùng nhóm: Không có.

| Khớp               | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?)                        |
| ------------------- | ---: | --: | ----: | -------------------------------------------------------------- |
| Chưa có dữ liệu |   — |  — |    — | Chưa có thư mục nhãn của bạn cùng nhóm để so sánh. |
| Chưa có dữ liệu |   — |  — |    — | Chưa có thư mục nhãn của bạn cùng nhóm để so sánh. |

Luật mới cần giữ trong `GUIDELINE_MINI.md`:

- Khi một khớp bị vật thể che nhưng còn trong khung ảnh, xác định vị trí từ hai khớp lân cận và gán `v=1`; chỉ gán `v=0` khi khớp nằm ngoài biên ảnh.

## 4. Model

| Chỉ số       | yolo26n-pose gốc | Sau fine-tune |  Chênh |
| -------------- | ----------------: | ------------: | ------: |
| pose_mAP50     |            0.8450 |        0.8450 |  0.0000 |
| pose_mAP50-95  |            0.6853 |        0.6908 | +0.0055 |
| pose_precision |            0.9734 |        0.9792 | +0.0058 |
| pose_recall    |            0.8462 |        0.8462 |  0.0000 |
| box_mAP50-95   |            0.8119 |        0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` tăng 0.0055, từ 0.6853 lên 0.6908. Mức tăng nhỏ cho thấy 20 ảnh bổ sung ít thông tin mới so với model gốc; chúng có thể giúp model thích nghi nhẹ với các tư thế bị che bởi xe, bàn hoặc dụng cụ, nhưng chưa đủ lớn để kết luận khả năng dùng thực tế.
2. Sau fine-tune, `box_mAP50-95` là 0.8041 còn `pose_mAP50-95` là 0.6908, chênh 0.1133. Model tìm người dễ hơn tìm từng khớp vì bbox chỉ cần bao phủ cơ thể, còn keypoint phải định vị chính xác các khớp nhỏ có thể bị che hoặc ngoài khung.
3. Output hiện có chỉ lưu ảnh prediction đã render, không có JSON theo từng keypoint hoặc một nhãn lỗi theo từng ảnh. Cần đối chiếu thủ công `outputs/runs/predictions/test/` với ảnh test gốc trước khi gọi chính xác loại lỗi; chưa có bằng chứng đủ để kết luận một lỗi cụ thể.
4. Không có output OKS theo từng ảnh giữa nhãn và model. `eval_vs_gold.json` là giữa nhãn của tôi và gold, trong đó `train_13.jpg` có OKS thấp nhất là 0.0 cho hai người bị thiếu hẳn. Không thể dùng số này để kết luận model hay nhãn nào đúng hơn.
5. Chưa thể xác định vì notebook chưa lưu phép ghép prediction–ground truth theo từng ảnh. Với nhãn, `train_13.jpg` là ảnh cần ưu tiên xem lại vì thiếu 2 người; cần chạy hoặc bổ sung đánh giá per-image của model để so sánh công bằng.

## 5. Một rule evidence bạn đã dùng

Ở `train_06.jpg`, người đi xe máy có `right_ear` nằm sau mũ bảo hiểm/góc quay đầu. Khớp này còn ở trong biên ảnh nên cần được đặt theo đường viền đầu và vị trí mắt–vai lân cận, với `v=1`, thay vì đặt `v=0`. Gold cho khớp này cho thấy lệch nhẹ 20 px, vì vậy cần chỉnh vị trí chứ không xóa khớp. Quy tắc này tách rõ “bị che nhưng có thể ước lượng” khỏi “ra ngoài khung”.
