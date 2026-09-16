# Mini guideline - nhóm: Nhóm 1  |  người gán: Vương Tuấn Dương  |  ngày: 2026-09-16

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

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Đặt chấm v=1 tại vị trí ước lượng giữa eo và đùi, dựa theo đường viền hông nhìn qua quần áo | Hông là khớp giải phẫu, không bao giờ nhìn thấy trực tiếp trên người mặc đồ. Dùng v=1 vì khớp vẫn còn trong khung ảnh, chỉ bị quần áo che |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Đặt chấm v=1 tại vị trí ước lượng của tai, dựa theo phần tai còn nhìn thấy hoặc vị trí đầu | Tai vẫn nằm trong khung ảnh, chỉ bị che bởi tóc/mũ. Có đủ bằng chứng từ phần đầu xung quanh để suy ra vị trí |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp nằm ngoài mép ảnh (gối, cổ chân) đặt v=0. Các khớp còn trong ảnh gán bình thường | Khớp thật sự nằm ngoài khung hình thì không có bằng chứng để đoán vị trí |
| Cổ tay nằm sau tay lái / sau thân mình | Đặt chấm v=1, ước lượng vị trí cổ tay dựa theo hướng cẳng tay | Cổ tay vẫn còn trong khung ảnh, bị che bởi vật thể nhưng vẫn suy ra được từ hướng cánh tay |
| Hai người chồng lên nhau | Gán đủ 17 điểm cho mỗi người. Khớp người sau bị người trước che thì v=1 | Mỗi người cần skeleton riêng. Khớp bị che bởi người khác vẫn suy ra được vị trí từ phần cơ thể còn thấy |
| Người nhỏ đến mức nào thì không gán nữa | Gán tất cả người nhìn thấy được trong bộ ảnh này vì ảnh đã được chọn sao cho mọi người đều đủ lớn | Bộ ảnh lab đã lọc sẵn, không có người quá nhỏ. Nếu phân vân, ghi lại ảnh vào mục 3 |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_06`, người thứ `1`, khớp `left_ear`

- Mơ hồ ở chỗ nào: Người quay lưng gần 90 độ, tai trái bị đầu che gần hết, chỉ thấy mờ đường viền
- Bạn quyết thế nào: Đặt v=2 (visible) vì vẫn nhận ra được vị trí tai
- Vì sao: Phần tai còn lộ ra đủ để xác định tâm khớp, không cần ước lượng
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu đặt v=0, model mất dữ liệu về vị trí tai khi người nghiêng, giảm khả năng dự đoán tai ở góc nhìn khó

### Ca 2 - ảnh `train_04`, người thứ `2`, khớp `left_hip`

- Mơ hồ ở chỗ nào: Người nằm nghiêng trên mặt đất, hông trái bị thân mình và quần áo che kín
- Bạn quyết thế nào: Đặt v=1 (occluded), ước lượng vị trí hông dựa theo đường eo và đùi
- Vì sao: Hông là khớp giải phẫu luôn bị che bởi quần áo, và ở đây còn bị thân mình che thêm, nhưng vẫn suy ra được vị trí từ phần thân và chân
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu đặt v=0, model không có dữ liệu hông cho tư thế nằm, sẽ kém khi gặp người nằm

### Ca 3 - ảnh `train_15`, người thứ `1`, khớp `nose`

- Mơ hồ ở chỗ nào: Người quay lưng hoàn toàn, không nhìn thấy mặt, mũi nằm phía bên kia đầu
- Bạn quyết thế nào: Đặt v=0 (outside) vì không có bất kỳ bằng chứng nào để xác định vị trí mũi
- Vì sao: Mũi nằm hoàn toàn ở mặt đối diện, không có cơ sở suy ra vị trí chính xác
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu đặt v=1 và đoán bừa vị trí, model học sai vị trí mũi khi người quay lưng, tạo bias hệ thống

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_hip` (bạn `6%` / họ `___%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Guideline chưa rõ - hông là khớp giải phẫu không nhìn thấy trực tiếp, cần thống nhất luật ước lượng
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Hông luôn là v=1 nếu người còn trong khung ảnh, đặt chấm tại điểm giữa eo và đùi, dựa theo đường viền cơ thể nhìn qua quần áo
