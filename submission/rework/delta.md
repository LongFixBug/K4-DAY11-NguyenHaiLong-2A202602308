# Rework delta

| zone | matched before | matched after | missing before | missing after | spurious before | spurious after |
|---|---:|---:|---:|---:|---:|---:|
| center | 5 | 6 | 2 | 1 | 5 | 4 |
| mid | 10 | 10 | 1 | 1 | 3 | 3 |
| edge | 2 | 2 | 0 | 0 | 1 | 0 |

## Findings action=rework
- adasind_019560.jpg L3 SPURIOUS: không áp dụng
- adasind_019560.jpg L5 SPURIOUS: không áp dụng
- adasind_019560.jpg L6+R5 WRONG_CLASS: không áp dụng
- adasind_019560.jpg L7+R6 WRONG_CLASS: không áp dụng
- adasind_019560.jpg R4 MISSING: không áp dụng
- adasind_014670.jpg L1 IGNORE_SCOPE: đã sửa
- adasind_032280.jpg L2 IGNORE_SCOPE: chưa sửa
- adasind_032280.jpg L1+R1 ATTRIBUTE: đã sửa
- adasind_034080.jpg L12 IGNORE_SCOPE: chưa sửa
- adasind_034080.jpg L10+R3 ATTRIBUTE: đã sửa
- adasind_034080.jpg L2+R9 WRONG_CLASS: đã sửa
- adasind_034080.jpg L6+R2 BOX_GEOMETRY: chưa sửa
- adasind_034080.jpg L8 SPURIOUS: chưa sửa
- adasind_034080.jpg L11 SPURIOUS: chưa sửa
- adasind_034080.jpg L2 SPURIOUS: đã sửa
- adasind_034080.jpg L6 SPURIOUS: đã sửa
- adasind_034080.jpg L8 SPURIOUS: chưa sửa
- adasind_034080.jpg L11 SPURIOUS: chưa sửa
- adasind_034080.jpg R2+M3 MISSING: chưa sửa
- adasind_034080.jpg R9 MISSING: đã sửa
- adasind_014670.jpg L1 IGNORE_SCOPE: đã sửa
- adasind_032280.jpg L1 IGNORE_SCOPE: đã sửa
- adasind_034080.jpg L12 IGNORE_SCOPE: chưa sửa
- adasind_034080.jpg L8 DUPLICATE: đã sửa
- adasind_034080.jpg L10 ATTRIBUTE: đã sửa
- adasind_032280.jpg L2 IGNORE_SCOPE: chưa sửa
- adasind_034080.jpg L12 IGNORE_SCOPE: chưa sửa
- adasind_034080.jpg L4+R2 BOX_GEOMETRY: chưa sửa
- adasind_034080.jpg L6 SPURIOUS: đã sửa
- adasind_034080.jpg L8 SPURIOUS: chưa sửa

## Nhận xét rework

- Số trước → sau (IoU 0.5, so với teaching reference): center matched 5 → 6, missing 2 → 1, spurious 5 → 4; mid giữ
  nguyên 10/1/3; edge spurious 1 → 0. Tổng matched 17 → 18, missing 3 → 2, spurious 9 → 7.
- Đã sửa có căn cứ: 014670 xóa box Bike trên tay lái ego (R07), bật truncated cho Bus mép trái (R05); 032280 xóa
  polygon ego_body phủ lên người lái xe máy áo caro và bỏ truncated sai (R10, R05); 034080 đổi Truck → ThreeWheeler
  (R04), xóa 3 polygon ego_body phủ lên xe thật (R10), bật truncated cho Pedestrian sát mép phải (R05).
- Giữ có lý do, không sửa theo reference: 014670 Bus (reference ghi Truck, xem ticket 2, `E0_reference_defect`) và
  ThreeWheeler h=55 reference sót; 032280 Bike h=43 (chờ rule R01b, `E2_guideline_gap`).
- Còn sót sau rework (ghi trung thực, cần vòng sau): box Bike trên tay lái ego ở 032280 [0,1283,243,1757] và 034080
  [0,1243,214,1803] (P0, R07); cặp box trùng ThreeWheeler/Car trên vật xa 034080 [334–387,1064–1109]; Car trắng
  034080 [468,1065,579,1155] chưa thu về phần nhìn thấy (R02). Dòng "không áp dụng" là finding của C0 (calib), không
  nằm trong slice rework.
- Một số dòng trong danh sách trên dùng chỉ số L của bản r1_craft, một số dùng chỉ số của v2 (findings vai `rework`),
  nên cùng một vật có thể xuất hiện hai lần với số L khác nhau.
