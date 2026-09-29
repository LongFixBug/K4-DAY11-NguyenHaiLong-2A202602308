# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Phạm vi ignore (`ego_body`) — cả 3 frame B1-mid + C0 | 3 IGNORE_SCOPE ở r1_craft (014670 L1, 032280 L2, 034080 L12) + 1 SPURIOUS ở calib (C0 L5); thêm 4 polygon ego_body phủ nhầm lên xe thật (032280, 034080, C0) | Lỗi P0 (R10): ignore sai làm các box thật bị bỏ khỏi phép so, số liệu phía sau không đáng tin | Overlay `qa_overlay.html`, XML đã khóa, polygon ego_body của reference để so vị trí |
| Vật nhỏ/xa ở center — frame 034080 | 5 SPURIOUS + 2 MISSING ở center (zone_table): trùng box L8/L11, class sai L2 (Truck→ThreeWheeler), box vẽ cả phần bị che L6 | Center có nhiều lỗi L nhất (5 spurious/7 ref); lỗi class ThreeWheeler cũng là lỗi chính của model nên dễ lan nếu dùng pre-label | Crop phóng to từng vật, cặp L/R/M trong `model_compare.html`, `local_quality_conflicts.csv` |

Giới hạn của kết luận từ ba frame ADASIND: chỉ 3 frame liền cảnh, một camera gắn xe máy, 20 vật reference (edge chỉ
có 2), teaching reference có thể sai (014670 R5). Số đo mô tả độ khớp với reference trên slice này, không phải tỉ lệ
lỗi của annotator hay model trên toàn dữ liệu.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv`: lấy mẫu phân tầng theo camera × normal/hard, rồi trong mỗi
ô rải theo scene/chuyến đi và thời điểm (ngày/đêm, thời tiết) — mỗi scene tối đa vài frame, cách nhau đủ xa (ví dụ
≥2 giây hoặc khác đoạn đường) để không đếm các frame liền nhau như ca độc lập. Kiểm bảng đếm theo class hiếm
(ThreeWheeler, rider), zone (center/mid/edge) và vùng seam để chắc mỗi ô hard có đủ ca khó. Kế hoạch này chỉ giúp
**tìm** ca cần soi: vì ô hard được chọn có chủ đích (oversample), tỉ lệ lỗi đo trên 200 frame không đại diện cho
50.000 frame; muốn ước lượng tỉ lệ lỗi cần một mẫu ngẫu nhiên riêng có trọng số.
