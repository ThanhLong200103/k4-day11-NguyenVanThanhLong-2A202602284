# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B4 | MISSING | 2 |
| center | B4 | SPURIOUS | 6 |
| center | B4 | WRONG_CLASS | 2 |
| center | C0 | SPURIOUS | 2 |
| edge | B4 | SPURIOUS | 1 |
| edge | C0 | WRONG_CLASS | 1 |
| mid | B4 | ATTRIBUTE | 2 |
| mid | B4 | BOX_GEOMETRY | 2 |
| mid | B4 | IGNORE_SCOPE | 2 |
| mid | B4 | MISSING | 8 |
| mid | B4 | SPURIOUS | 11 |
| mid | B4 | STRUCTURE | 1 |
| mid | B4 | WRONG_CLASS | 2 |

## Top defects
- SPURIOUS: 20 (ví dụ frame adasind_019560.jpg)
- MISSING: 10 (ví dụ frame adasind_265065.jpg)
- WRONG_CLASS: 5 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: Lỗi nổi bật nhất là **SPURIOUS ở zone mid (11 dòng)**, nhưng phần lớn là của **model** (`M_only`, E4_model_domain): model tách người lái/người ngồi sau xe máy thành Pedestrian riêng (249480 M2, M4; 261480 M2, M5 — 4 ca, trái R03) và gọi ThreeWheeler là Car/Truck (261480 M6, M8, M10, M12 — cả 3/3 auto-rickshaw trong frame). Vì lặp lại ở nhiều frame và nhiều vật nên tôi xếp E4 (hệ thống), không phải lỗi ngẫu nhiên. Về phía **người gán (L)**, lỗi thật tập trung ở frame 265065 (lề đường đông): người ngồi trên xe tay ga bị gán Pedestrian thay vì Bike (L7, E1, R03), bỏ sót người đứng trên nền tối quầy hàng (R8, E1, R01) và box lẻ trong cụm xe đỗ (L1, IGNORE_SCOPE, P0). Hai ca L còn lại giữ nhãn: 261480 L2 (E0 — reference thiếu xe thứ hai, model M11 cũng thấy) và 265065 L5 (E2 — R01 không nói H đo phần nhìn thấy hay toàn thân).
- Cách sửa và ai nhận việc (`owner`): (1) `annotator` (tôi) đã rework 5 ca P0/P1 ở P5 — delta zone mid: matched 10→12, missing 3→1, spurious 2→1 (`rework/delta.md`). (2) `ai_team`: escalate lỗi ThreeWheeler→Car và tách rider (bổ sung dữ liệu auto-rickshaw/xe máy chở 2 người, thêm luật hậu xử lý gộp Pedestrian nằm trọn trong Bike). (3) `guideline`: làm rõ R04 cho van thùng kín chở hàng và R01 cho vật bị che (`20_guideline_patch.md`). (4) `qa`: báo reference owner về 261480 R2 (thiếu xe thứ hai) và ca R7/265065 chưa giải quyết (E5).
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): `screenshots/265065_rider_scooter.png` (L7 Pedestrian → Bike, R03); `screenshots/261480_threewheeler_vs_model.png` (model Car/Truck trên 3 ThreeWheeler, R04); `screenshots/265065_van_class.png` (van thùng kín, R04 chưa rõ); `screenshots/261480_second_bike.png` (xe thứ hai mà reference thiếu). Dòng findings: r3_diag 261480 `L3+R5` (escalate), 265065 `L7`, `R5+M7`, `R8+M5`, `L1` IGNORE_SCOPE (rework), 265065 `L2` WRONG_CLASS (escalate), 261480 `L2` (E0).
