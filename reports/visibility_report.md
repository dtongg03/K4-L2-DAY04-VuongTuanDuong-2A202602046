# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.9 khớp có v > 0 mỗi người
- Tổng: v=2 391 | v=1 70 | v=0 32

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 8 | 0 | 28% |
| 1 | left_eye | 18 | 10 | 1 | 34% |
| 2 | right_eye | 20 | 9 | 0 | 31% |
| 3 | left_ear | 16 | 12 | 1 | 41% |
| 4 | right_ear | 21 | 7 | 1 | 24% |
| 5 | left_shoulder | 28 | 1 | 0 | 3% |
| 6 | right_shoulder | 29 | 0 | 0 | 0% |
| 7 | left_elbow | 27 | 2 | 0 | 7% |
| 8 | right_elbow | 27 | 0 | 2 | 0% |
| 9 | left_wrist | 23 | 5 | 1 | 17% |
| 10 | right_wrist | 24 | 1 | 4 | 3% |
| 11 | left_hip | 26 | 3 | 0 | 10% |
| 12 | right_hip | 27 | 2 | 0 | 7% |
| 13 | left_knee | 23 | 3 | 3 | 10% |
| 14 | right_knee | 23 | 1 | 5 | 3% |
| 15 | left_ankle | 18 | 4 | 7 | 14% |
| 16 | right_ankle | 20 | 2 | 7 | 7% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
