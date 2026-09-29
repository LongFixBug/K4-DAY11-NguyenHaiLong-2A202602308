# QA review · B1-mid

Mã khóa: AA43-F1C0

**QA reviewer: Nguyễn Hải Long (NguyenHaiLong)** · **QC: Nguyễn Như Quỳnh (NguyenNhuQuynh)** · Annotator: Trần
Minh Hiếu (TranMinhHieu).

Repo này đã chạy `mode` ở chế độ solo trước khi chốt nhóm nên giữ slice B1-mid (xem `40_decision_log.csv` D6).
Bản khóa (mã AA43-F1C0) được giao cho Nguyễn Hải Long. Các nhận xét dưới đây được lập từ cold review trên bản đã
khóa, đối chiếu `qa_overlay.html` với rules v1.0.0, trước khi mở teaching reference; Nguyễn Hải Long đã soát và
thống nhất giữ nguyên làm kết quả QA. Chỉ nêu vi phạm luật nhìn thấy trên ảnh, không suy nguyên nhân.

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_014670.jpg | L1 | R07 | Box `Bike` [0,1094,335,1774] ở góc dưới trái là tay áo, tay lái, chân người lái xe ego — phải là `ignore_region` reason `ego_body`, không phải box. |
| adasind_032280.jpg | L2 | R07 | Box `Bike` [0,1283,243,1757] cũng là tay/tay lái xe ego; đã có polygon `ego_body` bao nó → box nằm trong ignore. |
| adasind_032280.jpg | L1 | R10 | Polygon `ego_body` [483,948,647,1271] phủ lên người lái xe máy áo caro (Bike L1) — ignore che nhầm một vật thật; thêm `truncated=true` trong khi xe nằm trọn trong vòng kính (R05). |
| adasind_034080.jpg | L12 | R07 | Box `Bike` [0,1243,214,1803] là tay lái ego, cần xóa. Ba polygon `ego_body` khác phủ lên Truck L2, Bike L1, Bike L13 (vật thật trên đường). |
| adasind_034080.jpg | L8 | R01 | L8 ThreeWheeler và L11 Car gần trùng hoàn toàn trên cùng một vật xa (h≈44) — một vật hai box, hai class. |
| adasind_034080.jpg | L10 | R05 | Pedestrian sát mép phải (x tới 1078) bị vòng kính/khung cắt nhưng `truncated=false`. |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
