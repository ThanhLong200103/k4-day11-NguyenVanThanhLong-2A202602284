# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| **Cảnh lề đường đông, zone mid** (vd `adasind_265065.jpg`) | 5/6 khác biệt L–R của slice nằm ở frame này: 1 WRONG_CLASS rider→Pedestrian (L7/R5), 2 MISSING Pedestrian trên nền tối quầy hàng (R7, R8), 1 IGNORE_SCOPE cụm xe đỗ (L1), 1 Pedestrian sát ngưỡng H (L5) | Zone mid có n_ref lớn nhất (13) và gánh toàn bộ 3 missing của L; lỗi đến từ mật độ + nền tối + vật chồng nhau, sẽ lặp lại ở mọi cảnh chợ/cửa hàng | Ảnh gốc + crop tăng tương phản; `screenshots/265065_rider_scooter.png`; dòng findings r3_diag L7, R5+M7, R8+M5, R7, L1; quyết định D3, D4 |
| **Class phương tiện đặc thù: ThreeWheeler / van** (vd `adasind_261480.jpg`, `adasind_265065.jpg`) | Model: 3/3 ThreeWheeler → Car (M6, M8, M10) + 1 box Truck trùng (M12); người: 1 van thùng kín không có luật (L2/265065), 1 ThreeWheeler bị che khó đọc (L3/261480) | Đây là class dễ nhầm có hệ thống (E4 + E2), ảnh hưởng mọi frame Ấn Độ có auto-rickshaw; nếu prefill bằng model thì lỗi nhân lên | `screenshots/261480_threewheeler_vs_model.png`, `screenshots/265065_van_class.png`; `local_quality_confusion.csv`; Ticket 1–2 |

Giới hạn của kết luận từ ba frame ADASIND: chỉ 3 frame, 17 vật reference, **không có vật nào ở zone edge**, nên
không nói được gì về lỗi ở rìa vòng kính; slice được chọn theo tên `mid` nên phân bố zone lệch sẵn. Reference là
teaching reference (có ít nhất một ca nghi thiếu: 261480 R2), không phải gold. Center/mid/edge chỉ là khoảng cách
tới tâm vòng kính, không nói vật gần hay xa xe. Các tỷ lệ precision/recall 0.82 chỉ mô tả 3 frame này.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv`: (1) lập bảng đếm theo `camera_id × slice_type` và theo
các trục rủi ro đã thấy ở ADASIND — mật độ cảnh (đông/thưa), có ThreeWheeler/van hay không, có vật ở zone edge
hay không, sáng/tối — rồi kiểm mỗi ô có tối thiểu vài frame; ô trống thì đổi frame chứ không tăng tổng. (2) Chọn
frame theo **cảnh/clip**, không theo frame: gom frame liền nhau cùng cảnh (cùng clip, timestamp cách nhau vài giây)
thành một nhóm, mỗi nhóm lấy tối đa 1–2 frame; nhiều frame liền nhau trong cùng một cảnh **không** được tính là
nhiều ca độc lập vì chúng lặp cùng vật, cùng ánh sáng, cùng lỗi. (3) Với ca seam, chọn theo **cặp** frame cùng
timestamp ở hai camera kề nhau và đếm là một ca. Kế hoạch này chỉ giúp **tìm ca cần soi**: mẫu được chọn có chủ
đích (dồn vào hard), không ngẫu nhiên, nên tỷ lệ lỗi đo trên 200 frame bị thổi phồng và không suy ra tỷ lệ lỗi của
50.000 frame; muốn đo tỷ lệ lỗi cần một mẫu ngẫu nhiên phân tầng riêng.
