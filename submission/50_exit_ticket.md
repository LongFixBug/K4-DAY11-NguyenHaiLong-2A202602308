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
3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm? Ở adasind_014670.jpg, L4+M3 / R5:
   mình (và model) gọi xe vàng ở mép trái là `Bus`, reference gọi `Truck`. Mình không tự sửa theo reference mà ghi
   finding `E0_reference_defect` với bằng chứng trên ảnh (thân dài, hàng cửa sổ), đưa vào decision log D2 và
   escalation ticket 2 — nhưng thừa nhận `truncated=false` của L4 là lỗi mình. Nếu làm lại, mình sẽ đọc kỹ R07 và
   `sensor_context` trước khi vẽ để nhận ra vùng ego (tay lái xe máy) thay vì box `Bike`, không phủ `ego_body` lên xe
   khác, và chạy self-QC rồi xóa hết cảnh báo trước khi khóa.
