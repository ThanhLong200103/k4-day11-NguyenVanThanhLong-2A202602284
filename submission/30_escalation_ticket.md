# Escalation ticket

## Ticket 1 — Model nhầm ThreeWheeler thành Car/Truck (E4, ai_team)

- **Frame:** `adasind_261480.jpg` — L3+R5 (72,768)-(98,825), L4+R6 (98,762)-(133,842), L5+R4 (252,746)-(334,858);
  findings r3_diag `L3+R5` (action=escalate), `L4+R6`, `L5+R4`, `M6`, `M8`, `M10`, `M12`.
- **Ảnh chụp:** `submission/screenshots/261480_threewheeler_vs_model.png`
- **Expected impact:** 3/3 auto-rickshaw trong frame bị model gọi Car (M6, M8, M10), một chiếc còn thêm box Truck
  trùng (M12). Nếu dùng model làm prefill cho ADASIND, recall ThreeWheeler của prefill ≈ 0 và annotator phải sửa
  class từng box; nếu dùng cho cảnh báo thì loại phương tiện chậm/đổi hướng đột ngột bị hiểu là ô tô. Cùng frame
  model còn tách rider thành Pedestrian (4 ca ở 249480 và 261480) → số Pedestrian giả tăng.
- **Owner:** `ai_team`
- **Recommendation:** (1) Bổ sung ảnh auto-rickshaw/e-rickshaw (Ấn Độ, fisheye) vào tập huấn luyện và đo lại
  confusion ThreeWheeler↔Car; (2) thêm NMS liên lớp để không ra Car+Truck trên cùng vật; (3) hậu xử lý gộp
  Pedestrian nằm trọn trong box Bike (R03). Tạm thời: không dùng class ThreeWheeler/Car của model làm prefill.

## Ticket 2 — R04 chưa nói van thùng kín chở hàng (E2, guideline)

- **Frame:** `adasind_265065.jpg` — L2 Truck (191,720)-(279,849); findings r3_diag `L2` WRONG_CLASS (action=escalate).
- **Ảnh chụp:** `submission/screenshots/265065_van_class.png`
- **Expected impact:** L, R, M cùng chọn Truck nhưng không có luật đứng sau; annotator khác có thể chọn Car theo
  “van → Car”, gây bất đồng Car/Truck lặp lại ở mọi cảnh phố có van giao hàng.
- **Owner:** `guideline`
- **Recommendation:** duyệt R04a trong `20_guideline_patch.md` (van theo công dụng thân xe), bump `v1.1.0`.
