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

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- 

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. **`pose_mAP50-95` thay đổi bao nhiêu sau fine-tune? Nếu nó giảm, hãy giải thích: 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?**
   - Chỉ số `pose_mAP50-95` tăng nhẹ **+0.0055** (từ 0.6853 lên 0.6908), đồng thời `pose_precision` cũng tăng từ 0.9734 lên 0.9792 (+0.0058). 
   - Dù tập train chỉ gồm 20 ảnh, model không bị giảm hiệu năng nhờ việc nhãn đã qua rework chuẩn (không còn lỗi đảo trái/phải, các khớp che `v=1` được ước lượng nhất quán theo giải phẫu). 20 ảnh này dạy cho model khả năng định vị chính xác hơn ở các tư thế bị che khuất và người quay nghiêng. Tuy nhiên, `box_mAP50-95` giảm nhẹ -0.0078 do kích thước tập train quá nhỏ khiến phân bố bounding box bị co cụm (bias nhẹ về kích thước người của 20 ảnh).

2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?**
   - `box_mAP50-95` (0.8041) cao hơn `pose_mAP50-95` (0.6908) là **0.1133** (~11.33%).
   - Model tìm **người** dễ hơn tìm **khớp** rất nhiều. Nguyên nhân là vì bounding box bao quát toàn bộ cơ thể người với vùng diện tích lớn, có nhiều đặc trưng tổng thể rõ rệt (hình khối đầu, thân, màu sắc quần áo). Ngược lại, khớp (keypoint) chỉ là một điểm nhỏ vài pixel, dễ bị che khuất, biến dạng giải phẫu theo tư thế chuyển động phức tạp, và đòi hỏi độ chính xác cục bộ rất khắt khe.

3. **Ở mục 5, tìm một ảnh model đoán sai. Gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):**
   - Ở ảnh test `test_03.jpg` (người đi xe đạp bị che chân): Model bị **lệch nhẹ** ở khớp cổ chân do bị bàn đạp và nan hoa che khuất, đồng thời bị **trượt hẳn** ở khớp cổ tay do model bắt nhầm điểm tâm sang đầu nắm của ghi-đông xe đạp.

4. **Ở mục 6, ảnh nào có OKS thấp nhất giữa bạn và model? Ai đúng - và bạn dựa vào đâu để nói vậy?**
   - Ảnh có OKS thấp nhất giữa nhãn của tôi và model là ảnh `train_06.jpg` (người lái xe phân khối lớn màu vàng quay lưng).
   - **Tôi đúng**, vì người lái xe quay lưng hoàn toàn về phía sau, toàn bộ khuôn mặt nằm ở mặt trước và bị che khuất hoàn toàn bởi mũ bảo hiểm và đầu. Model dựa trên prior có sẵn của COCO nên đã "đoán mò" các điểm mũi và mắt trôi ra phía trước kính chắn gió xe máy (trượt hẳn). Nhãn của tôi xác định đúng các điểm mặt ở trạng thái bị che/không nhìn thấy theo bằng chứng thực tế của góc chụp.

5. **Trong `tools/evaluate_pose_annotations.py` bạn đã có OKS nhãn-của-bạn vs gold. Ảnh nào bạn gán tệ nhất *cũng* là ảnh model đoán tệ nhất? Nếu có, điều đó nói gì về ảnh đó?**
   - Có. Ảnh có OKS vs gold thấp nhất trong bài gán của tôi là `train_14.jpg` (OKS = 0.7281) và `train_15.jpg` (OKS = 0.8233), và đây cũng chính là những bức ảnh mà model gặp khó khăn và đạt OKS thấp nhất.
   - **Điều đó nói lên rằng**: Đây là những bức ảnh có độ bất định thị giác vốn có rất cao (intrinsic visual ambiguity). Ví dụ ở `train_14.jpg`, hai người đứng ở khoảng cách xa, độ phân giải thấp, thời tiết sương mù mờ ảo, lại đội mũ ô che kín mặt và mặc quần áo thụng. Khi bằng chứng thị giác bị suy giảm nghiêm trọng, cả con người lẫn mô hình AI đều không thể xác định vị trí khớp với độ tin cậy tuyệt đối.

## 5. Một rule evidence bạn đã dùng

Ảnh `train_04.jpg`, người thứ 1 (người mặc áo hoodie xám bên trái), khớp `left_elbow` và `left_wrist`: Cánh tay trái của người này vươn sang bên phải để vịn vào ghi-đông xe của người bên cạnh, phần cổ tay bị che khuất một phần bởi bàn tay đeo găng và thân xe. Dựa vào hướng đi của cẳng tay và góc gập khuỷu tay còn nhìn thấy rõ, tôi xác định vị trí khớp cổ tay trái nằm ngay vị trí tiếp giáp ghi-đông, quyết định chọn `v=1` (occluded) và đặt chấm ước lượng. Không chọn `v=0` vì toàn bộ cánh tay vẫn nằm hoàn toàn bên trong khung ảnh.
