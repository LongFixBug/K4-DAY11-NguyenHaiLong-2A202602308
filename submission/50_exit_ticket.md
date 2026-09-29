# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao? **Cần quy tắc riêng, không phải DUPLICATE.** DUPLICATE là hai box cho một vật *trên cùng một
   ảnh* (như 034080 L8/L11 của mình). Ở seam, mỗi camera thấy vật thật bằng hình chiếu riêng, nên mỗi ảnh có một box
   hợp lệ (có thể `edge` ở camera này, `mid` ở camera kia). Việc hợp nhất thành một object là quyết định của tầng
   output, cần timestamp, calibration và policy — không phải lỗi của annotator.
2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera. Giữ cùng track ID khi vẫn là cùng một vật quan
   sát được liên tục (kể cả bị che ngắn nếu guideline cho phép). Thêm keyframe khi hình học thay đổi lớn — ví dụ vật
   đi từ center ra edge và bị méo/cắt, box nội suy không còn bám. Đặt Outside khi vật rời trường nhìn hoặc nằm hẳn
   trong vùng ignore (lens_border/ego). Trước khi nối track qua hai camera cần: timestamp đồng bộ giữa camera,
   intrinsic + extrinsic để chiếu hai box về cùng không gian (BEV/toạ độ xe), vị trí/vận tốc khớp trong vùng chồng,
   và policy output nói rõ có hợp nhất ID hay không.
3. Nhìn lại cả buổi: một chỗ nhóm tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   nhóm đã phối hợp xử lý thế nào, và nếu làm lại slice này sẽ đổi gì trong cách làm?
   - **Bất đồng thực tế (adasind_014670.jpg, L4+M3 / R5):** Xe màu vàng ở mép trái ảnh. Nhãn người vẽ (Hiếu) và model (M3) cùng xác định là `Bus`, trong khi teaching reference (R5) gọi là `Truck`.
   - **Phối hợp giải quyết giữa 3 thành viên:**
     * *Nguyễn Hải Long (vai B - QA độc lập):* Trong pha QA mù, Long đối chiếu độc lập và phát hiện điểm mâu thuẫn này theo R04; Long không tự ý sửa theo reference mà giữ nguyên hiện tượng, đồng thời chỉ ra lỗi thiếu thuộc tính `truncated=true` của box L4 do bị vành kính cắt.
     * *Trần Minh Hiếu (vai A - Gán nhãn):* Giải thích căn cứ ban đầu khi vẽ dựa trên đặc điểm trực quan (thân xe dài, có dải cửa sổ hành khách nằm ngang, không có thùng chở hàng của xe tải), và nhận lỗi đã để sót `truncated=false`.
     * *Nguyễn Như Quỳnh (vai C - Điều phối & Chẩn đoán):* Mở báo cáo `compare.html` và `local_quality.md`, ghi nhận đây là `E0_reference_defect` gây ra 1 FP Bus và 1 FN Truck giả tạo; Quỳnh đưa ca này vào `40_decision_log.csv` (Quyết định D2) và soạn `30_escalation_ticket.md` (Ticket 2) gửi Lab Coach để xem xét cập nhật teaching reference, quyết định giữ nguyên class `Bus` có căn cứ.
   - **Bài học rút ra nếu làm lại:** Cả nhóm sẽ thống nhất kỹ hơn ở pha P0 về định nghĩa vùng `ego_body` của camera gắn trên xe máy (tránh nhầm lẫn vẽ box `Bike` lên tay lái xe chủ); Annotator sẽ chạy kỹ `selfqc` trước khi khóa bản vẽ; và nhóm sẽ tiếp tục duy trì quy trình QA mù độc lập để không bị thiên kiến bởi đáp án tham chiếu.
