# Báo cáo tự kiểm định (Self-Review)

Người gán: Vương Tuấn Dương   Người kiểm: Vương Tuấn Dương (Làm bài độc lập)   Ngày: 2026-09-16

## 1. Ghi chú hình thức thực hiện
Học viên thực hiện bài lab cá nhân (không ghép nhóm), thay quy trình kiểm chéo bằng quy trình **Tự kiểm tra đối soát 3 lượt (Self-Review)** theo hướng dẫn của Lab.

## 2. Kết quả tự rà soát và phát hiện lỗi nội bộ

| Ảnh | Người thứ | Khớp | Lỗi phát hiện | Thao tác tự khắc phục |
| --- | ---: | --- | --- | --- |
| `train_04.jpg` | 1 | Toàn bộ khớp tay/vai | Đảo trái/phải do nhầm hướng nhìn màn hình | Hoán đổi lại toàn bộ các cặp `left_` và `right_` theo trục giải phẫu cơ thể người |
| `train_04.jpg` | 1 | `right_hip` | Toạ độ vượt nhẹ mép ảnh ($y = 457.15 > 457$) | Điều chỉnh toạ độ $y$ về $456.9$ để đạt chuẩn định dạng $y \le 1.0$ |
| `train_13.jpg` | 2 | `left_ankle` | Toạ độ vượt mép ảnh ($y = 283.1 > 281$) | Điều chỉnh toạ độ $y$ về $281.0$ để nằm trọn trong khung ảnh |
| `train_11.jpg` | 1 | `right_wrist` | Khớp bị che khuất gán nhầm cờ $v=0$ | Đổi sang $v=1$ và chấm vị trí ước lượng theo hướng cẳng tay |

## 3. Kết luận sau tự kiểm định

- Lỗi dễ mắc phải nhất khi làm độc lập: Nhầm lẫn trái/phải ở người đối diện và có xu hướng lạm dụng cờ $v=0$ cho các khớp bị che khuất.
- Biện pháp khắc phục: Sử dụng công cụ `visualize_pose.py` để nhìn trực quan đường nối xương (xanh/cam) và `visibility_report.py` để kiểm soát số lượng cờ $v=1$. Sau khi hoàn tất tự kiểm, bài gán đạt **OKS = 0.924** (Xuất sắc) và **0 lỗi đảo trái/phải**.
