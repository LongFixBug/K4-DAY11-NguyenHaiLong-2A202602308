# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 7 | 2 | 5 | 3 | 7 | SPURIOUS (3) |
| mid | 11 | 1 | 3 | 5 | 7 | SPURIOUS (2) |
| edge | 2 | 0 | 1 | 0 | 1 | SPURIOUS (1) |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: **center** là nơi L lỗi nhiều nhất — 5
  spurious và 2 missing trên n_ref=7 (chủ yếu ở 034080: box trùng L8/L11 trên một vật xa, L2 gọi Truck thay vì
  ThreeWheeler, L6 vẽ cả phần bị che). **Mid** là nơi M gãy nhiều nhất — 5 missing và 7 thừa trên n_ref=11; center
  cũng có 7 M thừa. Edge chỉ có 2 vật reference nên không đủ mẫu để kết luận.
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame: lỗi M ở center/mid
  không đến từ méo rìa mà từ **lệch taxonomy**: ThreeWheeler bị gọi Truck/Car ở cả 3 frame (014670 M6/M7, 032280
  M5/M9, 034080 M10/M11) và rider bị tách thành Pedestrian + Bike (032280 M3/M7, 034080 M7/M8/M12), trái R03/R04.
  M chỉ có một FP lớn liên quan fisheye: 032280 M8 Car h=1005 phủ thân xe ego và mặt đường méo. Lỗi L chủ yếu là
  phạm vi ignore (box trên ego, ego_body phủ lên xe thật) và phân loại vật nhỏ ở xa, không phải box lỏng ở rìa. IoU
  sweep cho thấy M ở center tụt từ 4 matched (IoU 0.5) xuống 1 (IoU 0.7) — box model lỏng hơn L. Giới hạn: chỉ 3
  frame, 20 vật reference, một camera và teaching reference có thể sai (014670 R5 Truck/Bus) — không suy tỉ lệ lỗi
  theo zone cho toàn bộ dữ liệu.
