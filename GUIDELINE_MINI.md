# Mini guideline - nhóm: 201  |  người gán: Trương Công Hoài Nam  |  ngày: 16/09/2026

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

| Tình huống                                        | Luật nhóm bạn chọn                                                                                                                                 | Vì sao                                                                                   |
| ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Hông của người mặc quần áo dài                    | Chấm ước lượng tại vị trí xương chậu, dựa vào thắt lưng và hướng của thân người; nếu quần áo che vị trí nhưng hông còn trong khung thì dùng `v=1`. | Tránh dùng `v=0` chỉ vì không nhìn thấy bề mặt hông.                                     |
| Tai bị tóc hoặc mũ bảo hiểm che một phần          | Ước lượng vị trí tai theo mắt, đầu và tai còn lại; nếu tai còn trong khung nhưng bị che thì đặt chấm và dùng `v=1`.                                | Phân biệt bị che với ra ngoài khung.                                                     |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp chân nằm ngoài mép ảnh dùng `v=0` và không đặt chấm; các khớp vẫn nằm trong phần ảnh dùng `v=2` hoặc `v=1` theo mức che.                  | `v=0` chỉ dành cho khớp thực sự ngoài ảnh, không dùng cho toàn bộ người.                 |
| Cổ tay nằm sau tay lái / sau thân mình            | Dựa vào hướng của cẳng tay, khuỷu tay và vai để ước lượng cổ tay; nếu cổ tay còn trong khung thì đặt chấm tại vị trí ước lượng và dùng `v=1`.      | Không đoán theo hướng bàn tay khi phần cổ tay bị che; nếu ra ngoài khung mới dùng `v=0`. |
| Hai người chồng lên nhau                          | Gán trọn 17 điểm cho từng người, theo dõi liên tục từ đầu đến chân của cùng một người; điểm bị người kia che vẫn đặt chấm và dùng `v=1`.           | Giảm lỗi trộn keypoint giữa hai skeleton.                                                |
| Người nhỏ đến mức nào thì không gán nữa           | Vẫn gán nếu còn xác định được người và vị trí khớp; chỉ dùng `v=0` cho khớp nằm ngoài ảnh, không bỏ cả skeleton vì người nhỏ hoặc mờ.              | Bộ dữ liệu yêu cầu mọi người trong ảnh đều có skeleton.                                  |

Theo thống nhất của nhóm, phần này mô tả bằng luật bằng chứng nhìn thấy được; không
đính kèm ảnh CVAT.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_02`, người trong ảnh, khớp mặt (mũi/mắt/tai)

- Mơ hồ ở chỗ nào: Không nhận diện rõ được mặt người nên khó xác định chính xác vị trí và trái/phải của các khớp trên mặt.
- Bạn quyết thế nào: Dựa vào hướng của đầu, vị trí tương đối với thân và các điểm còn nhận ra được để ước lượng; khớp còn trong khung nhưng bị che thì đặt chấm và dùng `v=1`.
- Vì sao: Khớp vẫn thuộc cơ thể và còn trong ảnh; không dùng `v=0` chỉ vì không nhìn thấy rõ.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model có thể học vị trí mặt lệch hoặc học nhầm trái/phải của mắt, tai và mũi.

### Ca 2 - ảnh `train_13`, người trong ảnh, khớp toàn thân

- Mơ hồ ở chỗ nào: Người vẫn nhận ra được nhưng ảnh bị blur và quá nhỏ, nên khó xác định tâm chính xác của các khớp.
- Bạn quyết thế nào: Theo dõi liên kết giải phẫu từ vai đến khuỷu đến cổ tay và từ hông đến gối đến mắt cá; đặt chấm ở vị trí ước lượng, dùng `v=1` khi khớp bị blur/che nhưng còn trong khung.
- Vì sao: Blur làm giảm độ chắc chắn về vị trí, nhưng không đồng nghĩa khớp nằm ngoài ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model học các tọa độ nhiễu, làm pose bị trượt khỏi khớp thật và giảm độ chính xác khi gặp người nhỏ hoặc ảnh mờ.

### Ca 3 - ảnh `train_04`, người thứ nhất, khớp cổ tay/cánh tay bị che

- Mơ hồ ở chỗ nào: Cánh tay bị che nên không xác định chắc chắn cổ tay nằm về hướng nào.
- Bạn quyết thế nào: Lần theo vai, khuỷu tay và đoạn cẳng tay còn nhìn thấy để ước lượng cổ tay; nếu cổ tay còn trong khung thì đặt chấm tại vị trí hợp lý nhất và dùng `v=1`.
- Vì sao: Hướng bàn tay không đủ bằng chứng, nhưng vị trí cổ tay có thể suy ra từ chuỗi khớp liền kề; chỉ dùng `v=0` nếu cổ tay đã ra ngoài mép ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model có thể học cẳng tay/cổ tay sai hướng hoặc học một pose không liên tục về mặt giải phẫu.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_hip` (bạn `38%` / họ `21%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: guideline về hông của người mặc quần áo dài chưa đủ cụ thể; hai bên dùng ngưỡng khác nhau khi quyết định hông bị che.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Hông được đặt theo xương chậu, thắt lưng và hướng thân; nếu vị trí hông còn trong khung nhưng bị quần áo che thì luôn đặt chấm và dùng `v=1`, chỉ dùng `v=0` khi hông nằm ngoài mép ảnh.
