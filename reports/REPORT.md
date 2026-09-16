# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Vương Tuấn Dương   Nhóm: Nhóm 1   Ngày: 2026-09-16

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 391 / 70 / 32 |
| Thời gian trung bình mỗi ảnh | ~4.5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear`: 41% (12/29)
2. `left_eye`: 34% (10/29)
3. `right_eye`: 31% (9/29)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng một phần. Vùng đầu và mặt (tai, mắt) có tỉ lệ `v=1` cao do người trong ảnh thường xuyên đội mũ bảo hiểm, đeo kính râm hoặc quay nghiêng/quay lưng khiến tai và mắt bị che một phần hoặc che hoàn toàn nhưng vẫn suy ra được vị trí tương đối của hộp sọ. Ngoài ra, các khớp cổ tay (`left_wrist` 17%) và hông (`left_hip` 10%) cũng là những khớp đòi hỏi suy luận giải phẫu nhiều vì bị che bởi vật cầm nắm hoặc trang phục.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.899 | 0.924 |
| OKS@0.50 | 0.966 | 1.000 |
| OKS@0.75 | 0.931 | 0.966 |
| Lỗi `dao_trai_phai` | 1 | 0 |
| Lỗi `nham_nguoi` | 1 | 0 |
| Lỗi `xoa_khop_bi_che` | 1 | 1 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_04.jpg`, người thứ 1 (mặc áo hoodie xám bên trái): Hoán đổi lại toàn bộ các cặp điểm trái/phải (`left_` $\leftrightarrow$ `right_`) gồm mắt, tai, vai, khuỷu tay, cổ tay do bị gán nhầm theo góc nhìn màn hình thay vì theo cơ thể người.
- `train_04.jpg`, người thứ 1, khớp `right_hip`: Hiệu chỉnh toạ độ $y$ từ $457.15 \rightarrow 456.9$ để khớp nằm trọn trong chiều cao ảnh (457px), sửa lỗi toạ độ tràn biên.
- `train_13.jpg`, người thứ 2 (áo vàng), khớp `left_ankle`: Hiệu chỉnh toạ độ $y$ từ $283.1 \rightarrow 281.0$ để nằm trọn trong biên ảnh (281px), giải quyết lỗi không đạt chuẩn định dạng.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Lỗi đảo trái/phải xảy ra ở ảnh `train_04.jpg`, người thứ 1 (mặc áo hoodie xám bên trái). Đây là một ảnh có độ rõ ràng khá tốt, người ngồi đối diện người chụp. Tuy nhiên, sai sót xảy ra do lúc gán nhãn thao tác nhanh và phản xạ theo thị giác trực tiếp trên màn hình (thấy tay vươn sang bên phải màn hình thì chọn `right_`, tay buông bên trái màn hình thì chọn `left_`), quên mất nguyên tắc cốt lõi: **trái/phải phải tính theo cơ thể người trong ảnh**.

## 3. Kiểm chéo

Bạn cùng nhóm: Nguyễn Thành Long

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| `left_ear` | 41% | 15% | 26% | Guideline chưa rõ: bên bạn coi tai bị mũ/tóc che là `v=1` và ước lượng chấm, bên bạn Long để `v=0` |
| `left_hip` | 10% | 0% | 10% | Guideline chưa thống nhất về việc ước lượng hông người mặc quần áo dài |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- Tai bị che bởi mũ bảo hiểm/tóc nhưng vẫn xác định được hình dáng đầu: Đặt cờ `v=1`, chấm tại vị trí đối xứng hoặc ước lượng theo vành tai.
- Hông người mặc quần áo dài: Thống nhất đặt `v=1` tại vị trí khớp háng giải phẫu (ngay dưới thắt lưng), không để `v=0` nếu cơ thể vẫn nằm trong ảnh.

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.812 | 0.825 | +0.013 |
| pose_mAP50-95 | 0.584 | 0.591 | +0.007 |
| pose_precision | 0.795 | 0.810 | +0.015 |
| pose_recall | 0.742 | 0.751 | +0.009 |
| box_mAP50-95 | 0.620 | 0.622 | +0.002 |

### Trả lời năm câu hỏi ở cuối notebook

1. **`pose_mAP50-95` thay đổi bao nhiêu?**
   `pose_mAP50-95` tăng nhẹ khoảng +0.007. Dù tập train chỉ có 20 ảnh, dữ liệu chất lượng cao (không còn lỗi đảo trái/phải, các khớp che `v=1` được ước lượng nhất quán) giúp model thích nghi tốt hơn với các tư thế người che khuất phức tạp mà không làm thoái hóa trọng số gốc.

2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?**
   `box_mAP` (0.622) cao hơn `pose_mAP` (0.591). Model tìm người dễ hơn tìm khớp vì bounding box người là vùng bao lớn với nhiều đặc trưng ngữ cảnh tổng thể (quần áo, hình dáng đầu, thân), trong khi keypoint đòi hỏi độ chính xác cục bộ cấp độ vài pixel và dễ bị ảnh hưởng bởi che khuất hoặc xoay góc.

3. **Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43:**
   Ảnh `test_03.jpg`, người đi xe đạp bị che chân: Model bị **lệch nhẹ** ở khớp cổ chân do bị bàn đạp che khuất và **trượt hẳn** ở khớp cổ tay do nhầm với tay lái xe.

4. **Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?**
   Ảnh `train_06.jpg` (người lái xe máy quay lưng). Nhãn của tôi đúng hơn vì model tự động dự đoán các khớp mặt bị trôi ra phía trước kính chắn gió xe, trong khi người lái xe quay lưng hoàn toàn nên các điểm này thực tế bị che hoàn toàn ở phía đối diện.

5. **Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?**
   Có, đó là ảnh `train_06.jpg` và `train_15.jpg`. Điều này cho thấy đây là những bức ảnh có góc nhìn khó (người quay lưng, tư thế gập người, nhiều vật che khuất như xe máy, mũ bảo hiểm), tạo độ bất định cao cho cả người gán nhãn lẫn thuật toán AI.

## 5. Một rule evidence bạn đã dùng

Ảnh `train_04.jpg`, người thứ 1 (người mặc áo hoodie xám bên trái), khớp `left_elbow` và `left_wrist`: Cánh tay trái của người này vươn sang bên phải để vịn vào ghi-đông xe của người bên cạnh, phần cổ tay bị che khuất một phần bởi bàn tay đeo găng và thân xe. Dựa vào hướng đi của cẳng tay và góc gập khuỷu tay còn nhìn thấy rõ, tôi xác định vị trí khớp cổ tay trái nằm ngay vị trí tiếp giáp ghi-đông, quyết định chọn `v=1` (occluded) và đặt chấm ước lượng. Không chọn `v=0` vì toàn bộ cánh tay vẫn nằm hoàn toàn bên trong khung ảnh.
