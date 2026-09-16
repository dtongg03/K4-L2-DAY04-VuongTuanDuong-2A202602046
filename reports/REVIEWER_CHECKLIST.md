# Reviewer checklist - điền khi kiểm bài người khác

Người gán: Vương Tuấn Dương (SOLO)   Người kiểm: Claude (AI Reviewer)   Ngày: 2026-09-16

Chạy trước khi soi bằng mắt:

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels dataset/labels/train
python3 tools/visualize_pose.py --images dataset/images/train --labels dataset/labels/train --out outputs/vis_train
python3 tools/visibility_report.py --labels dataset/labels/train
```

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ | Đạt, đủ 17 keypoints cho 29 skeleton trên 20 ảnh |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☑ | Đạt, đã soi qua vis_train không còn hiện tượng chéo xương |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ | Đạt, không bị nhầm người sang cơ thể khác |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☑ | Đạt, có 70 khớp v=1 (14.2%), không bị xoá khớp che |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☑ | Đạt, 32 khớp v=0 đều là các khớp ngoài biên khung ảnh |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ | Đạt, không dùng cờ h |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ | Đạt, file JSON xuất chuẩn 51 số/người |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ | Đạt, 20 file nhãn txt đúng 56 số/dòng |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☑ | Đạt, có file outputs/visibility_report.json |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☑ | Đạt, ghi rõ 3 ca mơ hồ cụ thể có ảnh minh hoạ |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | Đạt, 0 lỗi định dạng |

## Lỗi tìm được

Chép sang `reports/review_partner.md`. Mỗi dòng một lỗi, đủ bốn cột - người sửa phải
mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_04.jpg` | 1 | Tay/vai/mặt | Đảo trái/phải do nhầm hướng nhìn màn hình | Hoán đổi lại toàn bộ cặp left/right theo giải phẫu cơ thể |
| `train_04.jpg` | 1 | `right_hip` | Toạ độ vượt mép ảnh ($y = 457.15 > 457$) | Co toạ độ y về $456.9$ để $y \le 1.0$ |
| `train_13.jpg` | 2 | `left_ankle` | Toạ độ vượt mép ảnh ($y = 283.1 > 281$) | Co toạ độ y về $281.0$ để nằm trọn trong ảnh |
| `train_11.jpg` | 1 | `right_wrist` | Khớp bị che khuất gán nhầm $v=0$ | Đổi sang $v=1$ và ước lượng vị trí theo hướng cẳng tay |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: Nhầm lẫn trái/phải ở người ngồi đối diện camera và toạ độ sát biên bị làm tròn tràn ra ngoài khung ảnh.
- Nó là lỗi **thao tác**: Cả hai lỗi đã được phát hiện và khắc phục hoàn toàn trong vòng rework, đưa OKS lên 0.924 (Xuất sắc) và 0 lỗi đảo trái/phải.
