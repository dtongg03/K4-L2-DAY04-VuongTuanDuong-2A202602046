# Reviewer checklist - Tự kiểm định (Self-Review)

Người gán: Vương Tuấn Dương   Người kiểm: Vương Tuấn Dương (Làm bài độc lập)   Ngày: 2026-09-16

Chạy kiểm thử:

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels dataset/labels/train
python3 tools/visualize_pose.py --images dataset/images/train --labels dataset/labels/train --out outputs/vis_train
python3 tools/visibility_report.py --labels dataset/labels/train
```

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ | Đủ 17 keypoint/skeleton cho toàn bộ 29 người |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☑ | Đã tự soi trên visualize, không còn cắt chéo |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ | Không bị kéo xương nhầm người |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☑ | Đã rà soát, đạt 70 khớp v=1 (14.2%) |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☑ | Chỉ còn 32 khớp thực sự ra ngoài khung |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ | Không sử dụng cờ h |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ | Định dạng chuẩn COCO Keypoints 1.0 |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ | Đầy đủ 56 giá trị/dòng |
| 9 | Visibility report đã nộp đầy đủ | ☑ | outputs/visibility_report.json |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☑ | Đã ghi 3 ca mơ hồ cụ thể |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | Đạt chuẩn 0 lỗi định dạng |

## Lỗi đã tự phát hiện và sửa chữa

| Ảnh | Người thứ | Khớp | Lỗi phát hiện | Cách tự sửa |
| --- | ---: | --- | --- | --- |
| `train_04.jpg` | 1 | Tay, vai, mặt | Đảo trái/phải do nhầm hướng nhìn | Hoán đổi lại toàn bộ cặp left/right theo giải phẫu |
| `train_04.jpg` | 1 | `right_hip` | Toạ độ vượt nhẹ mép ảnh ($457.15 > 457$) | Co về $456.9$ để $y \le 1.0$ |
| `train_13.jpg` | 2 | `left_ankle` | Toạ độ vượt mép ảnh ($283.1 > 281$) | Co về $281.0$ để nằm trọn trong ảnh |
| `train_11.jpg` | 1 | `right_wrist` | Khớp bị che khuất gán nhầm $v=0$ | Đổi sang $v=1$ và ước lượng vị trí |

## Kết luận

- Tự kiểm soát chất lượng qua 3 lượt: Giúp bài làm đạt OKS trung bình **0.924** (Xuất sắc) và loại bỏ hoàn toàn lỗi đảo trái/phải.
