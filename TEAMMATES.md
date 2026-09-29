# Thành viên và phân vai — Day11 SVM 360 Fisheye

## 1. Thông tin nhóm
- Khóa/lớp: K4
- Tên nhóm: K4-DAY11-NguyenHaiLong
- Repo Public: https://github.com/LongFixBug/K4-DAY11-NguyenHaiLong-2A202602308.git
- Máy giữ hồ sơ chính / người quản lý: Nguyễn Hải Long
- Slice chung lấy từ mode.json: B1-mid
- Tên định danh vai A dùng cho --self: TranMinhHieu
- Kênh trao đổi nội bộ: Nhóm chat lớp
- Đại diện nộp (vai C): Nguyễn Như Quỳnh
- Commit chốt bài: b35c0d8

## 2. Ba vai chính
| Vai | Họ và tên | MSSV | Tên định danh trong mode | Trách nhiệm | Bằng chứng đóng góp |
|---|---|---|---|---|---|
| A · Gán nhãn | Trần Minh Hiếu | 2A202602280 | TranMinhHieu | Parking/C0/slice B1-mid, self-QC, lock, rework | submission/parking, submission/p1_calib, submission/r1_craft, submission/rework |
| B · QA độc lập | Nguyễn Hải Long | 2A202602308 | long | Review trước reference, finding QA, kiểm lại ca sửa | submission/r2_qa/qa_review.md, submission/findings.csv (r2_qa) |
| C · Chẩn đoán & điều phối | Nguyễn Như Quỳnh | 2A202602298 | quynh | Báo cáo, phân xử, kế hoạch, tích hợp, check và nộp | submission/r3_diag, submission/40_decision_log.csv, submission/manifest.json |

## 3. Bàn giao theo pha
| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | C → A, B | mode.json, slice B1-mid, team.json | Đã chốt vai và slice B1-mid chung | Hoàn thành |
| P2 · Khóa bản đầu | A → B, C | XML, lock.txt, slice B1-mid, AA43-F1C0 | Đã kiểm 33 box, 13 polygon, hash khớp | Hoàn thành |
| P3 · Chốt QA mù | B → C, A | qa_review.md, findings.csv, screenshot | Đã kiểm 6 vi phạm luật, đối chiếu qa_overlay | Hoàn thành |
| P4 · Quyết định sửa | C → A, B | findings r3_diag, 40_decision_log.csv | Đã đối chiếu ảnh, luật và phân xử ca Bus/Truck/ego | Hoàn thành |
| P5 · Kiểm bản sửa | A → B → C | annotations-v2.xml, lock2.txt, delta.md | Đã kiểm delta matched/missing/spurious | Hoàn thành |
| P6 · Chốt nộp | A, B → C | manifest.json, 50_exit_ticket.md | check exit 0, failed_gates rỗng | Hoàn thành |

## 4. Bất đồng và phối hợp
- Một ca đã phân xử: 014670 L4 xe vàng mép trái — reference ghi Truck nhưng ảnh thấy thân dài, hàng cửa sổ xe buýt; nhóm thống nhất giữ Bus, bật truncated=true và mở escalation (D2).
- Ca còn mở: D5 (vạch bãi đỗ dài #9 chạy ngang) ghi nhận quan sát trong observations.md.
- Đóng góp của A/B/C vào kế hoạch và exit ticket: A hoàn thiện nhãn và ảnh minh chứng; B đóng góp phần nhận xét và review; C tổng hợp 3 câu hỏi exit ticket, sampling 200 frame và gold set plan.
- Thay đổi phân công nếu có: Giữ nguyên phân công A (Hiếu), B (Long), C (Quỳnh).

## 5. Xác nhận trước khi nộp
- [x] A xác nhận nhãn và export đúng phiên bản: Trần Minh Hiếu (r1_craft/annotations.xml, rework/annotations-v2.xml)
- [x] B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: Nguyễn Hải Long (r2_qa/qa_review.md)
- [x] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: Nguyễn Như Quỳnh (manifest.json)
- [x] manifest.json tại commit chốt có failed_gates rỗng.
- [x] Repo nhóm Public, ảnh và các bằng chứng mở được.
- [x] C đã push và gửi link repo nhóm + commit qua kênh lớp công bố.
