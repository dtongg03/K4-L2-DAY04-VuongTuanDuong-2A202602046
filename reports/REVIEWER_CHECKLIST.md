# Reviewer checklist - điền khi kiểm bài người khác

Người gán: Nguyễn Thành Long   Người kiểm: Vương Tuấn Dương   Ngày: 2026-09-16

Chạy trước khi soi bằng mắt:

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels <bài của họ>
python3 tools/visualize_pose.py --images dataset/images/train --labels <bài của họ> --out /tmp/vis_review
python3 tools/visibility_report.py --labels dataset/labels/train --compare <bài của họ>
```

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ | Đủ 17 keypoint/skeleton |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☑ | Đã kiểm trên visualize |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ | Không bị kéo xương sang người khác |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☑ | Đã sửa các trường hợp bị che |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☑ | Chuẩn quy định COCO |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ | Không sử dụng cờ h |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ | Định dạng 51 phần tử/người |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ | Đủ 56 trường số/dòng |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☑ | Có file outputs/visibility_report.json |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☑ | Đã điền 3 ca mơ hồ |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | Đạt chuẩn 0 lỗi |

## Lỗi tìm được

Chép sang `reports/review_partner.md`. Mỗi dòng một lỗi, đủ bốn cột - người sửa phải
mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_01.jpg` | 1 | `left_hip` | Để `v=0` dù người đứng trọn trong ảnh | Đổi sang `v=1` và ước lượng vị trí khớp hông |
| `train_04.jpg` | 2 | `right_ear` | Để `v=0` khi đội mũ bảo hiểm | Đổi sang `v=1`, chấm ước lượng vị trí tai |
| `train_11.jpg` | 1 | `left_wrist` | Bị bàn che khuất để `v=0` | Đổi sang `v=1` theo hướng cẳng tay |
| `train_15.jpg` | 1 | `left_ankle` | Đảo trái/phải do người cúi quay lưng | Đổi chéo lại hai cổ chân theo trục cơ thể |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: Khớp bị che (vật cản, quần áo, mũ) thường bị đánh `v=0` thay vì `v=1`.
- Nó là lỗi **guideline chưa rõ** giữa việc mất dấu vết hoàn toàn và bị che khuất một phần trong khung ảnh.
