# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Vương Tuấn Dương   Nhóm: Nhóm 1   Ngày: 2026-09-16

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 31 |
| v=2 / v=1 / v=0 | 405 / 14 / 108 |
| Thời gian trung bình mỗi ảnh | ~5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear: 6% (2/31)
2. right_ear: 6% (2/31)
3. left_wrist: 6% (2/31)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng, tai và cổ tay là hai vùng hay bị che nhất. Tai thường bị tóc hoặc mũ che một phần, khó phân biệt giữa "nhìn thấy mờ" (v=2) và "bị che nhưng suy ra được" (v=1). Cổ tay thì hay nằm sau vật cầm tay hoặc sau thân mình. Hông (left_hip, right_hip cũng 6%) là khớp giải phẫu không bao giờ nhìn trực tiếp trên người mặc quần áo, nên luôn phải ước lượng.

## 2. Chấm với gold

<!-- Chưa có gold - sẽ điền sau khi protected release được mở -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | | |
| OKS@0.50 | | |
| OKS@0.75 | | |
| Lỗi `dao_trai_phai` | | |
| Lỗi `nham_nguoi` | | |
| Lỗi `xoa_khop_bi_che` | | |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- (Chờ gold để điền)
-
-

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

<!-- Chờ gold để điền -->

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- (Chờ kiểm chéo để điền)

## 4. Model

<!-- Chờ chạy Colab để điền -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | | | |
| pose_mAP50-95 | | | |
| pose_precision | | | |
| pose_recall | | | |
| box_mAP50-95 | | | |

### Trả lời năm câu hỏi ở cuối notebook

1. (Chờ chạy Colab)

2. (Chờ chạy Colab)

3. (Chờ chạy Colab)

4. (Chờ chạy Colab)

5. (Chờ chạy Colab)

## 5. Một rule evidence bạn đã dùng

Ảnh `train_04`, người thứ 2 (nằm trên mặt đất), khớp `left_hip`. Người này nằm nghiêng nên hông trái bị thân mình và quần áo che kín hoàn toàn. Tuy nhiên, dựa vào vị trí eo (nơi thân trên gặp quần) và hướng đùi trái, tôi xác định được vị trí hông trái nằm giữa hai điểm đó. Do đó tôi chọn v=1 (occluded) và đặt chấm tại vị trí ước lượng. Không chọn v=0 vì hông vẫn nằm trong khung ảnh, chỉ bị che chứ không ra ngoài mép.
