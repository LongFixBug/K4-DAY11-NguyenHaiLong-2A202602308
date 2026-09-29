# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh): (1) polyline #2, đoạn sơn trắng tiền cảnh giữa đáy ảnh,
  x≈403–527, y≈651–719, chia hai ô đỗ ở hàng gần camera; (2) polyline #3, đoạn sơn tiền cảnh bên phải, x≈695–954,
  y≈621–683, song song với #2 và là ranh giới ô kế tiếp. Các polyline còn lại (#5–#8, #10–#27) là các đoạn chia ô
  ngắn của hàng giữa và hàng xa, vẽ theo phần sơn nhìn thấy, dừng khi vạch mờ.
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: vệt vàng/mảng sơn nhỏ trên mặt đường ở đáy ảnh (x≈340–390,
  y≈712–720) và mép bãi tiếp giáp hàng rào (y≈465) — không tạo ranh giới một ô đỗ riêng lẻ, chỉ là dấu mặt đường/biên
  bãi nên không gán `parking_line`.
- Polygon `free_space` dừng ở đâu; có phần bị che nào không: polygon #28 phủ mặt lối xe trống tiền cảnh (y≈509–720,
  hết chiều ngang ảnh), dừng ở đáy khung; polygon #29 phủ dải mặt đường hàng xa (y≈462–538). Cần tránh xe đỏ đỗ ở
  x≈193–221, y≈457–478 — polygon #29 phải đi vòng qua xe, không xuyên qua.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”): polyline #9 (x 0–959, y≈507–540) là vạch dài chạy
  ngang cả bãi. Nó vừa là đầu các ô hàng giữa vừa có thể là biên lối xe chạy; theo `docs/11` một dải dài dẫn lối xe
  không phải `parking_line`, nên đây là ca cần QC Nguyễn Như Quỳnh quyết định (ghi trong `40_decision_log.csv` D5).
