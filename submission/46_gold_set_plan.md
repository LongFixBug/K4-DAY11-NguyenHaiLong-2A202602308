# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Rider xe máy, xe ba bánh, người băng qua đường, ngược sáng/đêm (35 frame) | Nhầm class ThreeWheeler↔Truck/Car và tách rider thành Pedestrian (thấy ở cả người và model trong ADASIND B1-mid) | Ảnh fisheye gốc (không undistort), intrinsic + vòng kính theo từng camera, timestamp đồng bộ | Hai annotator độc lập gán cùng frame; khác class/IoU<0.5 → reviewer thứ ba phân xử theo rule_id; normal (25) review riêng với hard |
| rear | Người/vật thấp sát đuôi xe ở rìa méo, thân xe ego/móc kéo ở đáy (30 frame) | Vật ở rìa bị cắt, gần ngưỡng H=40; dễ box nhầm thân xe ego hoặc phủ ignore lên vật thật (lỗi thật ở bài lab) | Mask `ego_body` và `lens_border` đo riêng cho camera sau; extrinsic để biết vùng seam | Soát riêng phạm vi ignore trước khi soát box; kiểm ≥1 frame không có ego để tránh ép vẽ ego |
| left | Seam trước-trái/sau-trái, xe máy vượt sát hông, gương ego che (25 frame) | Một vật xuất hiện trên hai camera → dễ bị gọi DUPLICATE hoặc bỏ sót ở camera kia | Extrinsic trái + timestamp để ghép ca seam; ghi rõ không ghép box khi thiếu calibration | Reviewer xem song song frame cùng timestamp của camera kề trước khi quyết định; normal (20) review riêng |
| right | Seam trước-phải/sau-phải, curb/vạch ô đỗ khi đỗ song song, người bước ra giữa xe đỗ (25 frame) | Vật bị che một phần (occluded) và bị cắt (truncated) cùng lúc; nhầm biên curb với vạch ô đỗ | Extrinsic phải, calibration curb/ground plane nếu làm BEV | Hai người gán độc lập; bất đồng ghi vào decision log với rule_id; chỉ vào gold khi đồng thuận hoặc đã phân xử |

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule): khi thay camera/ống kính hoặc vị trí lắp (vòng
  kính và vùng ego đổi), khi calibration cập nhật, khi `rules_version` tăng (ví dụ v1.1.0 từ `20_guideline_patch.md`),
  hoặc khi phân bố dữ liệu đổi (mùa, thành phố, loại xe mới). Frame cũ phải được gán lại hoặc gắn version rule cũ.
- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box: một xe máy ở góc trước-phải xuất hiện ở
  `edge` của camera front và ở `mid` của camera right cùng timestamp. Chỉ ghép thành một vật/một track khi có
  timestamp đồng bộ, extrinsic của hai camera để chiếu về cùng không gian (BEV/xe) và policy output nói rõ đầu ra là
  box per-camera hay object hợp nhất. Thiếu một trong ba → giữ hai box hợp lệ, không gọi DUPLICATE.
- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera: bài
  lab chỉ có một camera ADASIND (gắn trên xe máy), 3 frame, 20 vật reference; ngay teaching reference cũng có ca đáng
  ngờ (014670 R5 Truck/Bus). Hai người đồng ý với nhau vẫn có thể cùng sai rule; camera sau/trái/phải có vùng ego,
  góc nhìn và seam khác hẳn, nên cần reviewer độc lập và mẫu riêng cho từng camera.
