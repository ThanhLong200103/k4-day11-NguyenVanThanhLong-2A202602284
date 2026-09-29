# Sensor context

Frame đã xem: slice B4-mid (`adasind_249480.jpg`, `adasind_261480.jpg`, `adasind_265065.jpg`) và hai frame đối
chiếu `adasind_006840.jpg`, `adasind_271039.jpg`. Ảnh dọc 1080×1920.

- **Vị trí camera (quan sát):** Có vẻ là camera gắn **thấp, hướng về phía trước (hơi lệch sang trái)** trên một
  xe nhỏ/hở (kiểu xe ba bánh hoặc xe máy chở người), không phải ô tô kín. Dựa trên: (1) mặt đường chiếm nửa dưới
  ảnh, vạch kẻ đứt chạy ra xa về điểm tụ ở giữa-trái (rõ ở `249480`, `006840`, `271039`); (2) các xe phía trước
  chạy cùng chiều, lề đường và nhà cửa nằm bên phải; (3) góc nhìn thấp gần mặt đường. Không thấy mui xe, cản
  trước/sau hay gương, nên đây chỉ là suy đoán.

- **`ego_body` (thân xe ego):** Thứ thuộc về xe ego nhìn thấy được là **chân / dép của người ngồi trên xe** ở **góc
  dưới-trái** ảnh, không phải capo hay gương:
  - `adasind_249480.jpg`: ống quần + dép ở mép trái từ khoảng y ≈ 970 xuống y ≈ 1830 (x ≈ 0–380), thêm bàn chân
    ở đáy vòng kính (x ≈ 250–380, y ≈ 1710–1830) — chiếm khoảng 10% vùng ảnh hữu ích.
  - `adasind_261480.jpg`: chân + dép ở góc dưới-trái, x ≈ 0–300, y ≈ 1400–1780.
  - `adasind_265065.jpg`: mũi dép + bàn chân ở góc dưới-trái, x ≈ 0–260, y ≈ 1470–1760 (nhỏ hơn hai frame trên).
  - **Không thấy** ở `adasind_006840.jpg` (góc dưới-trái chỉ có bóng đổ trên mặt đường) và `adasind_271039.jpg`
    (góc dưới là mặt đường trống). Vậy `ego_body` **không xuất hiện ở mọi frame**, và vị trí/kích thước thay đổi
    theo tư thế người ngồi.

- **Vòng kính / vành đen:** Vùng ảnh hữu ích là **hình tròn** đường kính gần bằng chiều ngang ảnh (~1080 px), bị
  khung hình cắt nhẹ ở mép trái và phải. Vì ảnh dọc nên **vành đen dày nhất ở trên và dưới** (mỗi phía khoảng
  150–250 px) và ở bốn góc; hai bên trái/phải chỉ còn dải đen mỏng. Vòng kính chiếm khoảng 75–80% chiều cao khung.
  Vị trí tâm **không cố định hoàn toàn**: mép trên vòng kính ở khoảng y ≈ 80 (`271039`), ≈ 140 (`006840`),
  ≈ 200–240 (slice B4-mid), nên cần soát `lens_border` import sẵn theo từng frame. Ở mép vòng kính còn có viền
  sáng tím/xám và vệt lóa (rõ ở `265065`, `271039`).

- **Biến dạng:** Giữa ảnh gần như thẳng; càng ra rìa càng cong mạnh (barrel). Dây điện ở trời bị uốn thành vòng
  cung (`261480`, `006840`), nhà và cột điện ở mép phải nghiêng/ngả ra ngoài (`265065`, `271039`), vạch kẻ và mép
  đường gần mép ảnh bị cong. Vật ở rìa (ví dụ người đi xe máy sát mép phải trong `261480`) bị kéo giãn, to bất
  thường so với vật cùng khoảng cách ở giữa, và dễ bị vòng kính cắt mất một phần (`truncated`).

- **Giới hạn:** Dữ liệu chỉ có **một camera**, không có ảnh bốn camera của một hệ SVM. Không có calibration, độ cao
  lắp, góc nhìn hay thông số rig; mọi nhận định về vị trí camera ở trên chỉ là suy đoán từ ảnh. Tôi mới xem 5 frame
  trong 48 ảnh, nên các quan sát này chưa chắc đại diện cho toàn bộ dữ liệu.
