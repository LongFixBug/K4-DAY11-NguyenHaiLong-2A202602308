# Sensor context

- Rig: một camera fisheye đơn, ảnh dọc 1080×1920, gắn trên xe máy (ego là xe hai bánh), nhìn về phía trước theo
  chiều xe chạy. ADASIND không kèm tài liệu rig; mô tả này suy từ quan sát trên các frame slice B1-mid và C0. Đây chỉ
  là **một** camera, không đại diện bốn camera front/rear/left/right của SVM, không có calibration hay timestamp để
  ghép seam giữa camera.
- `ego_body`: góc dưới bên trái vòng kính — tay/cánh tay người lái, tay lái và phần đầu xe máy (khoảng x 0–340,
  y 1050–1800 trên các frame 014670, 032280, 034080, 019560); ở 014670 thấy thêm chân người lái.
- Vòng kính: hình tròn gần giữa khung, tâm khoảng (455–605, 895–925), bán kính khoảng 770–812 px theo
  `assets/frames.csv`; vòng tròn bị cắt ở hai cạnh trái/phải (đường kính > chiều rộng 1080) và chiếm khoảng 80–85%
  diện tích khung hình. Phần ngoài vòng (trên, dưới, bốn góc) là vành đen `lens_border`.
