# Review partner - Kiểm chéo bài bạn cùng nhóm

Người gán: Nguyễn Thành Long   Người kiểm: Vương Tuấn Dương   Ngày: 2026-09-16

## Kết quả kiểm

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_01.jpg` | 1 | `left_hip` | Để `v=0` dù người đứng trọn trong ảnh | Đổi sang `v=1` và ước lượng vị trí khớp hông theo đường eo |
| `train_04.jpg` | 2 | `right_ear` | Để `v=0` khi đội mũ bảo hiểm | Đổi sang `v=1`, chấm ước lượng vị trí tai dưới vành mũ |
| `train_11.jpg` | 1 | `left_wrist` | Bị bàn che khuất để `v=0` | Đổi sang `v=1` theo hướng cẳng tay |
| `train_15.jpg` | 1 | `left_ankle` | Đảo trái/phải do người cúi quay lưng | Đổi chéo lại hai cổ chân theo trục cơ thể |

## Nhận xét

- Lỗi lặp đi lặp lại nhiều nhất: Khớp bị che (bởi quần áo, phụ kiện mũ hoặc vật cản) thường xuyên bị gán thành `v=0` (Outside) thay vì `v=1` (Occluded) có chấm ước lượng.
- Đó là lỗi **guideline chưa rõ**: Chưa thống nhất kỹ ranh giới giữa việc "khớp mất hoàn toàn dấu vết giải phẫu" và "khớp còn trong khung hình nhưng bị che khuất một phần". Sau khi thảo luận, nhóm đã thống nhất cập nhật vào `GUIDELINE_MINI.md`.
