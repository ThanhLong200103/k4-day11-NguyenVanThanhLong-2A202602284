# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh):
  - **Vạch 1 – tiền cảnh, giữa-trái:** đoạn sơn trắng chéo từ khoảng (406, 653) xuống tới mép dưới ảnh tại (527, 719). Polyline dừng ở mép dưới vì phần sơn còn lại nằm ngoài khung hình.
  - **Vạch 2 – tiền cảnh, bên phải:** đoạn sơn trắng chéo từ khoảng (700, 623) chạy ra mép phải ảnh tại (959, 684). Dừng ở mép phải vì vạch bị khung hình cắt.
  - Vì sao chúng chia ô: hai đoạn là vạch ngắn, song song nhau, cùng hướng chéo và cách nhau đều bằng bề rộng một xe; đầu trên của mỗi vạch kết thúc gọn (không nối vào dải sơn dài nào). Đó đúng là dạng vạch ngăn giữa hai ô đỗ cạnh nhau, không phải vạch dẫn hướng xe chạy.

- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao:
  - **Loại dải sơn ngang dài ở giữa ảnh** (khoảng y ≈ 510–545, kéo gần hết chiều ngang bãi). Dải này chạy liên tục qua nhiều ô, nối đầu các vạch ô ở hàng xa lại với nhau; nó là ranh giữa các hàng đỗ / mép lối xe chạy, không tự tạo ranh giới cho một ô riêng lẻ, nên không gán `parking_line`.
  - Cũng không vẽ đoạn sơn đứng sát mép trái dưới (x ≈ 27, y ≈ 680–720) vì chỉ thấy một mẩu ngắn bị khung cắt, không xác định được nó thuộc ô nào; và vệt màu nhạt ở (≈ 340–390, 712–720) là vết bẩn/vá mặt đường, không phải sơn.

- Polygon `free_space` dừng ở đâu; có phần bị che nào không:
  - Polygon nằm ở tiền cảnh, giữa hai vạch đã vẽ: cạnh trái bám Vạch 1, cạnh phải bám Vạch 2, cạnh dưới là mép dưới ảnh (y = 720) và một đoạn mép phải (x = 960).
  - Cạnh trên dừng ở đường nối hai đầu trên của vạch (≈ (406, 650) → (700, 624)), tức chỗ vạch sơn kết thúc; phía trên đó mặt đường vẫn trống nhưng không còn vạch để bám ranh nên tôi không kéo polygon lên.
  - Không có xe, curb hay vật cản nào che vùng này. Chiếc xe đỏ duy nhất ở xa (≈ 195–220, 457–478) và cột đèn bên phải đều nằm ngoài polygon. Vùng chỉ bị giới hạn bởi khung hình ở phía dưới và bên phải.

- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”):
  - `free_space` hiện là phần mặt đường trống **bên trong** khoảng giữa hai vạch ô ở tiền cảnh, trong khi hướng dẫn định nghĩa `free_space` là lối xe chạy. Tôi không chắc vùng này nên tính là lối xe chạy hay là các ô đỗ trống; nếu cần đúng định nghĩa lối xe chạy thì polygon nên chuyển lên dải mặt đường trống giữa hàng vạch tiền cảnh và dải sơn ngang (y ≈ 545–620).
  - Dải sơn ngang dài ở giữa ảnh: tôi xếp nó là ranh hàng / lối xe chạy, nhưng nếu quy ước của đội coi vạch đầu ô (stall end line) cũng là `parking_line` thì cần gán thêm.
