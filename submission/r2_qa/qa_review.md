# QA review · B4-mid

Mã khóa: 8EBE-518E

**Đây là cold review cá nhân sau khoảng nghỉ 15 phút** (lab chạy một người, không có bạn đổi chéo). Trong pha này
chưa mở reference, model overlay hay worked HTML của slice B4-mid; chỉ dùng `qa_overlay.html` + ảnh gốc
`assets/images/` phóng to 3–4×. Mỗi nhận xét trích `object_ref` theo overlay (L = nhãn đã khóa).

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_261480.jpg | L3 | R04 | Box ThreeWheeler (72,768)-(98,825), zone mid: chỉ thấy đuôi xe màu xám với biển số bị làm mờ, phần lớn bị người lái áo xanh (L2) che. Không thấy rõ mui/bánh ba bánh nên class ThreeWheeler chưa chắc — có thể là Car nhỏ. Cần người thứ hai xem lại class. |
| adasind_265065.jpg | L2 | R04 | Box Truck (191,720)-(279,849), zone center: xe màu kem, cabin hẹp, thùng kín cao phía sau. Có thể là van chở hàng (→ Truck) hoặc van chở người (→ Car theo R04). Luật R04 không nói rõ van thùng kín chở hàng, cần escalate nếu reference khác. |
| adasind_261480.jpg | L2 | R02 | Box Bike người áo xanh (35,765)-(68,893): nửa dưới xe bị Bike L8 che hoàn toàn, nhưng đáy box kéo xuống y=893 — có nguy cơ đã vẽ cả phần bị khuất (trái R02 "bám phần nhìn thấy"). Kiểm lại đáy box có nên dừng ở y≈865. |
| adasind_265065.jpg | L5 | R01 | Box Pedestrian (279,765)-(298,815) cao ~50 px, sát ngưỡng H=40 và bị xe L2 che một phần (occluded=true đúng). Giữ box vì ≥40 px, nhưng đây là ca dễ bất đồng giữa người gán nhãn. |
| adasind_261480.jpg | L8 | R05 | Bike rìa trái (0,750)-(57,929) chạm biên khung x=0 → truncated=true là đúng (bị khung cắt, không phải bị vật khác che). Không lỗi, ghi lại để xác nhận R05 được áp dụng đúng. |
| adasind_249480.jpg | L2 | R03 | Xe máy chở 2 người (608,716)-(824,998) = một box Bike bao cả hai người và xe, đúng R03. Không có box Pedestrian thừa. Không lỗi. |

Tóm tắt: 0 lỗi P0 (lens_border/ego_body đúng ở cả 3 frame), 3 ca cần xem lại (L3/261480 class, L2/265065 class,
L2/261480 hình học), 1 ca sát ngưỡng. Các dòng tương ứng đã ghi vào `findings.csv` với `round=r2_qa`,
`cell=L_only`, `why` để trống.
