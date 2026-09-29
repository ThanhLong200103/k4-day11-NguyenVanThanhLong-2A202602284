# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao?
   **Không phải `DUPLICATE`**, cần quy tắc seam riêng. `DUPLICATE` là hai box cho một vật **trên cùng một ảnh**.
   Ở seam, mỗi camera là một ảnh gốc khác nhau (R02: box bám phần nhìn thấy trên ảnh gốc), nên mỗi camera phải có
   box riêng của nó — box ở camera này có thể là `edge` + `truncated`, ở camera kia là `mid` gần đủ. Xoá một box
   sẽ làm thiếu nhãn ở camera đó. Việc “hai box là một vật” là câu hỏi **liên kết** (cùng object ID giữa hai
   camera), thuộc policy output (ví dụ BEV hợp nhất), không phải lỗi gán nhãn trên từng ảnh. Policy cần nói: giữ cả
   hai box, gắn chung một cross-camera ID khi đủ bằng chứng, camera nào là chính cho output hợp nhất.

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.
   Giữ **cùng track ID** khi vẫn là cùng vật và còn quan sát được liên tục (kể cả khi bị che một phần, đi qua vùng
   méo ở rìa). Thêm **keyframe** khi hình học thay đổi lớn mà nội suy không bám được: vật đi từ mid ra edge và bị
   kéo giãn/cong, bị cắt bởi vòng kính (bật `truncated`), đổi hướng, hoặc bắt đầu/hết bị che. Đặt **Outside** khi
   vật ra khỏi trường nhìn hữu ích (qua vành kính, vào `lens_border`/`ego_body`) hoặc bị che hoàn toàn; nếu quay
   lại thì theo guideline task mở lại cùng ID khi chắc là cùng vật, không thì ID mới. Trước khi **nối track qua hai
   camera** cần: timestamp đồng bộ giữa hai camera (độ lệch đã biết), calibration intrinsics + extrinsics để chiếu
   vị trí vật về cùng hệ toạ độ xe/BEV và kiểm hai box rơi cùng chỗ, vùng seam đã xác định, và **chính sách output**
   (ID toàn cục hay ID theo camera, camera nào ưu tiên). ADASIND chỉ có một camera, không có calibration, nên trong
   lab này **không** được nối track qua camera.

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?
   `adasind_261480.jpg` **L2** — Bike người áo xanh sau người áo đen ở rìa trái. Reference chỉ có một Bike (R2) cho
   cả vùng nên compare báo L2 SPURIOUS. Tôi quay lại ảnh gốc (zoom 4.5×, `screenshots/261480_second_bike.png`)
   thấy tay lái/gương riêng của xe thứ hai, và model độc lập cũng có M11 Bike ở đúng chỗ. Tôi **giữ box**
   (E0_reference_defect, keep_with_reason, decision D2) nhưng chấp nhận góp ý QA của chính mình: đáy box đã kéo vào
   phần bị che nên rework thu đáy về y=868 (R02). Nếu làm lại: (a) soát cảnh đông ở lề đường bằng crop tăng tương
   phản ngay từ đầu — tôi bỏ sót người trên nền tối quầy hàng (265065 R8); (b) áp dụng R03 cho **mọi** người gần
   xe hai bánh, hỏi “người này ngồi trên xe không?” trước khi chọn Pedestrian (265065 L7); (c) vẽ `crowd_or_group`
   cho cụm xe đỗ không tách được thay vì box lẻ (265065 L1).
