# Rework delta

| zone | matched before | matched after | missing before | missing after | spurious before | spurious after |
|---|---:|---:|---:|---:|---:|---:|
| center | 4 | 4 | 0 | 0 | 1 | 1 |
| mid | 10 | 12 | 3 | 1 | 2 | 1 |
| edge | 0 | 0 | 0 | 0 | 0 | 0 |

## Findings action=rework
- adasind_265065.jpg L7 ATTRIBUTE: đã sửa
- adasind_265065.jpg L1 ATTRIBUTE: đã sửa
- adasind_249480.jpg L1 STRUCTURE: đã sửa
- adasind_265065.jpg L1 IGNORE_SCOPE: đã sửa
- adasind_265065.jpg L7 SPURIOUS: đã sửa
- adasind_265065.jpg R5 MISSING: đã sửa
- adasind_265065.jpg R8 MISSING: đã sửa
- adasind_265065.jpg L7 SPURIOUS: đã sửa
- adasind_265065.jpg R5+M7 MISSING: đã sửa
- adasind_265065.jpg R8+M5 MISSING: đã sửa
- adasind_261480.jpg L2 BOX_GEOMETRY: đã sửa
- adasind_265065.jpg L1 IGNORE_SCOPE: đã sửa

## Giải thích
Zone mid cải thiện (matched 10→12, missing 3→1, spurious 2→1) nhờ sửa 265065 L7→Bike (R03), thêm Pedestrian R8
(R01), và chuyển cụm xe đỗ bên trái thành `crowd_or_group` (R09). Phần còn lại **không** sửa cho giống reference
vì có căn cứ giữ nhãn: missing R7/265065 (E5, chưa đủ bằng chứng là người), spurious L2/261480 (E0, xe thứ hai mà
model M11 cũng thấy) và spurious center L5/265065 (E2, R01 chưa rõ cách đo H với vật bị che).
