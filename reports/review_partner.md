# Review partner - Kiểm chéo bài bạn cùng nhóm

Người gán: Vương Tuấn Dương (SOLO)   Người kiểm: Claude (AI Reviewer)   Ngày: 2026-09-16

## Kết quả kiểm

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_04.jpg` | 1 | Tay/vai/mặt | Đảo trái/phải do nhầm hướng nhìn màn hình | Hoán đổi lại toàn bộ cặp left/right theo giải phẫu cơ thể người |
| `train_04.jpg` | 1 | `right_hip` | Toạ độ vượt nhẹ mép ảnh ($y = 457.15 > 457$) | Co toạ độ y về $456.9$ để đạt chuẩn định dạng $y \le 1.0$ |
| `train_13.jpg` | 2 | `left_ankle` | Toạ độ vượt mép ảnh ($y = 283.1 > 281$) | Co toạ độ y về $281.0$ để nằm trọn trong ảnh |
| `train_11.jpg` | 1 | `right_wrist` | Khớp bị che khuất gán nhầm $v=0$ | Đổi sang $v=1$ và ước lượng vị trí theo hướng cẳng tay |

## Nhận xét

- Lỗi lặp đi lặp lại nhiều nhất: Nhầm lẫn quy ước trái/phải theo hướng nhìn của người trong ảnh và toạ độ biên ảnh bị làm tròn tràn ra ngoài khung.
- Đó là lỗi **thao tác**: Đã được phát hiện và xử lý dứt điểm qua vòng rework, đưa OKS lên 0.924 (Xuất sắc) và 0 lỗi đảo trái/phải.
