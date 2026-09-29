# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 4 | 0 | 1 | 1 | 4 | SPURIOUS (1) |
| mid | 13 | 3 | 2 | 3 | 7 | MISSING (3) |
| edge | 0 | 0 | 0 | 0 | 1 | — |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: **mid** gãy nhiều nhất cho cả hai phía — L có 3 missing + 2 spurious trên n_ref=13 (center chỉ 1 spurious/4), M có 3 missing + 7 thừa ở mid so với 1 + 4 ở center. Edge có n_ref=0 nên không kết luận được gì về edge (chỉ 1 box thừa của model).
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame: lỗi L ở mid **không** do méo fisheye mà do cảnh đông ở lề đường frame 265065 (cụm xe đỗ, người ngồi trên xe tay ga, người đứng trên nền tối quầy hàng) — 5/6 khác biệt L nằm ở frame này; mean IoU TP 0.83 và IoU sweep chỉ đổi 1 match giữa 0.5→0.7 nên hình học box ổn. Lỗi M ở mid chủ yếu là tách rider thành Pedestrian, nhận ThreeWheeler thành Car và box trùng — lỗi class/NMS, không phải vị trí. Giới hạn: chỉ 3 frame, 17 vật reference, không có vật ở edge; center/mid/edge chỉ là khoảng cách tới tâm vòng kính, không nói vật gần hay xa xe; slice tên `mid` nên phân bố zone bị lệch sẵn.
