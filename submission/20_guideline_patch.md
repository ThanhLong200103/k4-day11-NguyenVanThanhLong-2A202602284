# Guideline patch

Xuất phát từ hai ca E2 trong `findings.csv`: `adasind_265065.jpg` L2 (WRONG_CLASS, escalate) và
`adasind_265065.jpg` L5 (SPURIOUS, keep_with_reason). Không sửa trực tiếp `docs/02-rules-vi.md`.

- **Rule mới đề xuất:**
  - **R04a — Van theo công dụng thân xe:** van **chở người** (có hàng ghế/cửa sổ bên hông) → `Car`; van hoặc xe
    cabin hẹp có **thùng kín chở hàng** (không cửa sổ thân sau, thùng cao hơn cabin) → `Truck`. Khi không nhìn thấy
    thân sau để phân biệt, chọn theo hình dáng thấy được và bật ca vào findings với `why=E5_unresolved`.
  - **R01a — Ngưỡng H cho vật bị che:** H=40 px đo trên **phần nhìn thấy** của vật trên ảnh gốc (khớp với R02 “bám
    phần nhìn thấy”), không ước lượng toàn thân phía sau vật che. Vật có phần nhìn thấy < 40 px thì không box, kể cả
    khi toàn thân ước lượng ≥ 40 px.
- **Áp dụng cho:** class `Car`/`Truck` (R04a); mọi class khi `occluded=true` (R01a); mọi zone.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** R04 chỉ nói “van chở người → Car” và “xe bán tải nhỏ
  → Truck”; xe kem cabin hẹp thùng kín ở 265065 (L2/R3/M1) không thuộc ca nào — cả ba nguồn cùng chọn Truck nhưng
  chỉ vì cảm giác, không có luật (`screenshots/265065_van_class.png`). R01 nói “vật cao ≥ 40 px” nhưng không nói
  đo phần thấy hay toàn thân: người áo trắng 265065 L5 thấy được ~43–50 px (nửa dưới khuất sau Truck) — tôi box,
  reference không box; hai cách đọc R01 đều hợp lý nên bất đồng không phải lỗi của một bên.
- **`rules_version` mới:** v1.0.0 → v1.1.0
- **Hiệu lực từ:** round kế tiếp sau khi guideline owner duyệt (các slice ADASIND gán mới và round rework sau đó);
  nhãn đã khóa ở r1_craft/rework của vòng này giữ nguyên, ghi chú `rules_version=v1.0.0` trong findings.
