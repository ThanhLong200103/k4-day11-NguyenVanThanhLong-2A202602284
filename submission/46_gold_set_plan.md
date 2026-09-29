# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

> Cập nhật sau P4. Bài học từ ADASIND B4-mid đưa vào kế hoạch: lỗi người gán tập trung ở **cảnh đông ven đường**
> (rider trên xe tay ga bị gán Pedestrian, người trên nền tối bị sót, cụm xe đỗ cần `crowd_or_group`); lỗi model là
> **class phương tiện đặc thù** (ThreeWheeler → Car) và **tách rider**; hai ca không có luật (van thùng kín, H của vật
> bị che). Vì vậy mọi camera đều có ca hard “cảnh đông + phương tiện đặc thù”, không chỉ ca “rìa fisheye”.

Phân bổ tạm: front 60 (25 normal / 35 hard), rear 50 (20/30), left 45 (18/27), right 45 (18/27) = 200.
Hard nhiều hơn normal ở mọi camera vì ngân sách review nên dồn vào nơi nhãn dễ lệch; normal giữ làm baseline.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Vật nhỏ ở xa (<40 px gần ngưỡng), người/xe cắt ngang ở rìa vòng kính, ngược sáng | Vật nhỏ khó quyết định có box hay không; ở rìa fisheye vật bị kéo giãn nên box dễ quá rộng hoặc bị cắt | Gán trên ảnh fisheye gốc (không undistort); giữ intrinsics + tâm/bán kính vòng kính của camera front và timestamp | Hai annotator gán độc lập, reviewer thứ ba xử lý bất đồng theo rule; ca vẫn mơ hồ → escalate, chưa đưa vào gold |
| rear | Vật rất gần cản sau, trẻ em/cột thấp, đêm và đèn pha xe sau | `ego_body` (cản sau) che một phần vật; vật gần bị méo mạnh; lóa làm mất biên | Ảnh gốc rear + polygon `ego_body`/`lens_border` riêng của camera rear (vị trí khác front) | Soát riêng `ego_body` từng frame; so box với frame trước/sau cùng track để kiểm độ ổn định |
| left | Vật ở vùng seam góc trước-trái / sau-trái, xe máy song song sát hông | Cùng một vật thấy trên hai camera với hai box và hai zone khác nhau; vật sát hông bị vòng kính cắt | Extrinsics left↔front/rear và timestamp đồng bộ để đối chiếu seam | Review cặp frame cùng timestamp của hai camera; chỉ đưa vào gold khi đã có policy seam (xem dưới) |
| right | Seam góc trước-phải / sau-phải, đỗ sát lề: vỉa hè, người đi bộ, vạch ô đỗ cong ở rìa | Lề/vỉa hè dễ bị gán nhầm; vạch ô đỗ và mép đường khó phân biệt khi bị cong | Extrinsics right↔front/rear, timestamp; quy ước `parking_line` như `docs/11-parking-lines-vi.md` | Như left; thêm kiểm tra vạch chia ô vs vạch lối xe chạy bởi reviewer đã làm bài parking |

## Normal slice và review độc lập theo camera

| camera_id | Normal case cần chọn | Ai review độc lập | Giải quyết bất đồng |
|---|---|---|---|
| front | Đường thẳng ban ngày, mật độ vừa, vật chủ yếu ở center/mid | Annotator A và B gán độc lập (không thấy nhãn của nhau, không thấy model); reviewer C chưa gán frame đó | C đối chiếu theo rule id; hai bên trích rule + ảnh crop; không giải được → `escalate` guideline, ca ra khỏi gold tới khi có rule |
| rear | Lùi/đỗ ban ngày, ít vật, `ego_body` (cản sau) ổn định | A/B như trên; reviewer C soát riêng polygon `ego_body`/`lens_border` của rear trước | Bất đồng về phạm vi ignore xử lý trước (P0), rồi mới tới class/box |
| left | Chạy thẳng, xe song song ở mid, không có vật ở seam | A/B + reviewer C; ca seam do một reviewer D xem đồng thời front/rear cùng timestamp | Ca seam chỉ vào gold khi có policy seam; bất đồng khác như front |
| right | Chạy thẳng, lề/vỉa hè rõ, không đỗ sát lề | A/B + reviewer C; reviewer đã làm bài parking soát vạch ô đỗ | Như left; ca vạch sơn mơ hồ ghi E2 và escalate |

Bất đồng được ghi vào decision log với `rules_version`; đồng thuận của A và B **không** tự động là đúng — reviewer C
vẫn kiểm trên ảnh gốc, vì ở ADASIND chính reference cũng có ca thiếu (261480 R2).

- **Khi nào cần refresh gold set (đổi camera, calibration hoặc rule):** khi thay camera/ống kính hoặc đổi vị trí
  lắp (vòng kính, `ego_body` đổi chỗ); khi calibration (intrinsics/extrinsics) được đo lại; khi guideline đổi
  version (ví dụ rule về ngưỡng 40 px, truncated/occluded, seam); khi dữ liệu mới có miền khác (đêm, mưa, thành
  phố khác). Refresh ít nhất phần hard slice của camera bị ảnh hưởng và ghi version rule gắn với gold.
- **Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box:** xe máy đi ở góc trước-phải xuất hiện
  cùng lúc ở rìa camera front (zone `edge`, bị cắt) và camera right (zone `mid`, thấy gần đủ). Cần policy: giữ box
  ở camera nào làm chính, có ghép thành một object ID không. Evidence cần có: timestamp đồng bộ giữa hai camera,
  extrinsics để chiếu vị trí, và track ID nhất quán qua vài frame. Chưa có policy thì giữ cả hai box, đánh dấu ca
  cần reviewer quyết định, không đưa vào gold.
- **Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:**
  lab chỉ có một camera ADASIND; đồng thuận giữa annotator trên camera đó chỉ nói nhãn nhất quán **trên góc nhìn
  đó**. Nó không kiểm được vùng seam, sai lệch timestamp/extrinsics, vị trí `ego_body`/vòng kính khác nhau ở rear
  và hai hông, hay việc hai người cùng sai theo một cách (đồng thuận ≠ đúng). Mỗi camera cần review riêng và ca
  cross-camera cần evidence riêng.
