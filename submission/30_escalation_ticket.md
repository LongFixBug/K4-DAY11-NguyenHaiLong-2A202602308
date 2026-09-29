# Escalation ticket

## Ticket 1

- **Frame:** adasind_014670.jpg (L7+R3, M6, M7), adasind_032280.jpg (L7+R2, L8+R6, M5, M9), adasind_034080.jpg
  (L7+R7, M10, M11); thêm 032280 M8 Car h=1005 [18,912,1037,1917]
- **Ảnh chụp:** `submission/screenshots/model_threewheeler.png` (chụp từ `r3_diag/model_compare.html`, frame 034080)
- **Expected impact:** Model YOLO26m gọi ThreeWheeler là Truck/Car ở cả 3 frame và tách rider xe máy thành
  Pedestrian + Bike. Nếu dùng model làm pre-label, annotator phải sửa class ở mọi xe ba bánh và gộp rider → tốn thời
  gian và dễ sót (bài lab: 7 LR_noM + 17 M_only trên 3 frame). M8 là FP lớn phủ thân xe ego/mặt đường méo.
- **Owner:** ai_team
- **Recommendation:** Không dùng class ThreeWheeler từ pre-label cho tới khi fine-tune; map lại output model
  (person trên motorcycle → Bike) theo R03; thêm mask ego_body để lọc FP kiểu M8; đánh giá lại trên nhiều frame
  hơn trước khi kết luận E4 lệch miền fisheye.

## Ticket 2

- **Frame:** adasind_014670.jpg, L4+M3 / R5, box [0,845,70,1100]
- **Ảnh chụp:** `submission/screenshots/ref_bus_truck.png` (chụp từ `r1_craft/compare.html`, frame 014670)
- **Expected impact:** Teaching reference gọi xe vàng mép trái là Truck, trong khi ảnh cho thấy xe buýt (thân dài,
  hàng cửa sổ); làm local-quality ghi 1 FP Bus + 1 FN Truck cho người gán đúng.
- **Owner:** qa — QA: Nguyễn Hải Long; QC xác nhận: Nguyễn Như Quỳnh
- **Recommendation:** QA Nguyễn Hải Long đối chiếu lại class R5 theo R04 trên ảnh gốc; QC Nguyễn Như Quỳnh xác
  nhận kết luận rồi chuyển Lab Coach sửa teaching reference và ghi version reference mới.
