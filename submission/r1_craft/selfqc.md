# Tự soát


## Checklist thủ công
- [x] Phạm vi H=40 và vật cần vẽ
- [x] lens_border và ego_body
- [x] Class sáu nhãn
- [x] Rider và Bike
- [x] Geometry trên ảnh fisheye gốc
- [x] truncated và occluded
- [x] Vật thiếu hoặc box trùng
- [x] ignore_region có reason
- [x] Tên task raw_fisheye và export CVAT 1.1

## Ghi chú soát tay (bản nháp r1-draft.zip)
- Phạm vi: đã soát 3 frame ở zoom 3–4×; bỏ các xe/người ở xa cao < 40 px (xe trắng rìa trái 249480 cao ~28 px, dãy auto xa bên trái 265065 cao ~36–38 px). Người thứ ba nghi ở cửa hàng 265065 (x≈745–770) thực ra là hàng hoá, không box.
- lens_border: 2 polygon import mỗi frame bám đúng vành đen, không sửa. ego_body: vẽ ở cả 3 frame (chân/dép người ngồi + sàn xe ở góc dưới-trái).
- Class: xe Omni trắng 261480 = van chở người → Car (R04); xe cao thùng kín 265065 (191–279) → Truck (xe tải nhỏ, R04 — còn nghi, ghi vào findings); xe chở người trên nóc 249480 → Truck.
- Rider: xe máy 2 người 249480 = một Bike; xe tay ga đỗ không người 265065 = Bike riêng.
- Attribute: **sửa** 2 lỗi thiếu `occluded` ở 265065 — Pedestrian (175–193, bị xe tay ga xám che nửa dưới) và Bike (112–150, bị người đi bộ 148–166 che). Bike sát mép phải 261480 và Bike rìa trái 261480 giữ `truncated`.
- Không box nào nằm ≥50% trong ignore (đã kiểm Bike 648–1080 ở 261480 với lens_border).
- Tên task sửa từ "Day11 · ADASIND · C0 · B4-mid · raw_fisheye" thành "Day11 · ADASIND · B4-mid · raw_fisheye"; export CVAT for images 1.1.

## Fill ratio (K12)
- adasind_249480.jpg box 1 mid: 0.825
- adasind_261480.jpg box 1 mid: 0.853
- adasind_261480.jpg box 8 mid: 0.748
- adasind_265065.jpg box 8 mid: 0.653
