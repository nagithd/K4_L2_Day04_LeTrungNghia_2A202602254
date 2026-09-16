# Mini guideline - nhóm: SOLO|  người gán: Lê Trung Nghĩa  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống                                                   | Luật nhóm bạn chọn                                                                             | Vì sao |
| -------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- | ------- |
| Hông của người mặc quần áo dài                         | vẫn để là nhìn thấy                                                                          |         |
| Tai bị tóc hoặc mũ bảo hiểm che một phần               | để occluded                                                                                      |         |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | dán nhãn phần nhìn thấy                                                                       |         |
| Cổ tay nằm sau tay lái / sau thân mình                    | dự đoán và đặt cờ occluded                                                                 |         |
| Hai người chồng lên nhau                                   | dán nhãn phần nhìn thấy gắn cờ occluded                                                     |         |
| Người nhỏ đến mức nào thì không gán nữa             | dùng threshold tiêu chuẩn là 32px nhưng quyết định theo chất lượng hình ảnh thực tế |         |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train06`, người thứ `Person 145`

- Mơ hồ ở chỗ nào: Người trong ảnh quay lưng và bị che mắt 1 nửa phần cơ thể
- Bạn quyết thế nào: dán nhãn toàn bộ, đặt cờ occluded cho phần bị che khuất
- Vì sao: vì phần cơ thể bị che khuất vẫn nằm trong khung hình và vẫn là bộ phân cơ thể của người
- Nếu người khác quyết ngược lại thì model học sai cái gì: sẽ học thiếu các bộ phận cơ thể

### Ca 2 - ảnh `train11`, người thứ `___`, khớp `ankle`

- Mơ hồ ở chỗ nào: phần thân dưới bị bởi bàn nền không biết phần nào còn trong ảnh phần nào bị cắt
- Bạn quyết thế nào: dự đoán nhãn cho phần thân dưới
- Vì sao: Vì phần bị che khuất vẫn nằm trong khung hình
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model có thể đoán sai tỉ lệ cơ thể, hoặc dự đoán thiếu phần cơ thể bị che khuất

### Ca 3 - ảnh `train13`

- Mơ hồ ở chỗ nào: có tổng 3 người xuất hiện trong khung hình nhưng chỉ có 1 người làm chủ thể chính, 2 người còn lại bị mờ
- Bạn quyết thế nào: label 1 người là chủ thể chính
- Vì sao: tập trung vào nội dung chính của ảnh
- Nếu người khác quyết ngược lại thì model học sai cái gì: Thì model train trên bộ dữ liệu của tôi sẽ gặp nhiều FN

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `______` (bạn `___%` / họ `___%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**:
- Luật mới bổ sung vào mục 2 sau khi thống nhất:
