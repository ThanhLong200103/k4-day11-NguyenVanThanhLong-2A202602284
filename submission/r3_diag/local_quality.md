# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `8ebe518ec6d3022bbfa16fa932e130e40a57b331e00f2966ac172498e8c01262`; slice `B4-mid`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_249480.jpg, adasind_261480.jpg, adasind_265065.jpg. Frame thiếu trong export: không.
TP=14; FP=3; FN=3; số lần đối chiếu=20; mean IoU của TP=0.830.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.700 | 0.940 | 0.800 |
| precision | 0.824 | 0.880 | 0.600 |
| recall | 0.824 | 0.880 | 0.600 |
| jaccard | 0.700 | 0.819 | 0.429 |
| dice | 0.824 | 0.880 | 0.600 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 4 | 1 | 1 | 0.900 | 0.800 | 0.800 | 0.667 | 0.800 |
| Car | 2 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Pedestrian | 3 | 2 | 2 | 0.800 | 0.600 | 0.600 | 0.429 | 0.600 |
| ThreeWheeler | 3 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Truck | 2 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_249480.jpg | 2 | 0 | 0 | 1.000 | 1.000 | 1.000 |
| adasind_261480.jpg | 7 | 1 | 0 | 0.875 | 0.875 | 1.000 |
| adasind_265065.jpg | 5 | 2 | 3 | 0.500 | 0.714 | 0.625 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|
| Bike | 4 | 0 | 0 | 0 | 0 | 1 |
| Car | 0 | 2 | 0 | 0 | 0 | 0 |
| Pedestrian | 0 | 0 | 3 | 0 | 0 | 2 |
| ThreeWheeler | 0 | 0 | 0 | 3 | 0 | 0 |
| Truck | 0 | 0 | 0 | 0 | 2 | 0 |
| <extra> | 1 | 0 | 2 | 0 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
