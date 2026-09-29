# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B1 | BOX_GEOMETRY | 1 |
| center | B1 | DUPLICATE | 1 |
| center | B1 | MISSING | 4 |
| center | B1 | SPURIOUS | 15 |
| center | B1 | WRONG_CLASS | 1 |
| center | C0 | SPURIOUS | 1 |
| center | C0 | WRONG_CLASS | 1 |
| edge | B1 | ATTRIBUTE | 2 |
| edge | B1 | IGNORE_SCOPE | 5 |
| edge | B1 | SPURIOUS | 3 |
| edge | C0 | SPURIOUS | 1 |
| mid | B1 | ATTRIBUTE | 1 |
| mid | B1 | IGNORE_SCOPE | 1 |
| mid | B1 | MISSING | 5 |
| mid | B1 | SPURIOUS | 11 |
| mid | B1 | WRONG_CLASS | 1 |
| mid | C0 | MISSING | 1 |
| mid | C0 | WRONG_CLASS | 1 |

## Top defects
- SPURIOUS: 31 (ví dụ frame adasind_019560.jpg)
- MISSING: 10 (ví dụ frame adasind_019560.jpg)
- IGNORE_SCOPE: 6 (ví dụ frame adasind_014670.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: SPURIOUS=31 là lỗi đứng đầu nhưng tách làm hai nguồn khác
  nhau. (a) 17/31 là `M_only` — model gọi ThreeWheeler là Truck/Car (014670 M6/M7, 032280 M5/M9, 034080 M10/M11)
  và tách rider thành Pedestrian (032280 M3/M7, 034080 M7/M8/M12) → `E4_model_domain` (lệch taxonomy, lặp ở cả 3
  frame nên không phải một box lệch đơn lẻ). (b) Phần của người (L) tập trung ở center 034080: box trùng L8/L11, gọi
  Truck cho ThreeWheeler (L2 vs R9), vẽ cả phần bị che (L6 vs R2) → `E1_annotator_error`. Lỗi nghiêm trọng nhất về
  mức độ là IGNORE_SCOPE=6 (P0): box `Bike` đặt trên tay lái xe ego (014670 L1, 032280 L2, 034080 L12, C0 L5) và
  `ego_body` phủ lên xe thật (032280 L1) — mình chưa nhận ra camera gắn trên xe máy → `E1` kèm khoảng trống R07
  (`E2`, xem `20_guideline_patch.md`). Một phần SPURIOUS/MISSING là do reference (014670 L6, R5 Bus/Truck → `E0`).
- Cách sửa và ai nhận việc (`owner`): annotator (mình) rework — xóa box ego, vẽ một `ego_body` đúng chỗ mỗi frame,
  sửa class ThreeWheeler, xóa box trùng, bám phần nhìn thấy (R02); guideline bổ sung R07b/R01b (v1.1.0); ai_team
  nhận ticket 1 về ThreeWheeler/rider/FP ego; qa (QA Nguyễn Hải Long) soát lại class reference 014670 R5 (ticket 2),
  QC Nguyễn Như Quỳnh xác nhận và phân xử các ca `E5_unresolved` (032280 L6, 034080 L4) trước khi đóng.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): `screenshots/model_threewheeler.png`,
  `screenshots/ref_bus_truck.png`; findings r3_diag 034080 L2/L8/L11/R9 (R04, R01), 014670 L7+R3 và M6 (R04), r2_qa
  014670 L1 / 032280 L1 / 034080 L12 (R07, R10); `local_quality.md`: TP=17 FP=9 FN=3, precision 0.654, recall 0.850,
  Truck/Bus precision 0.
