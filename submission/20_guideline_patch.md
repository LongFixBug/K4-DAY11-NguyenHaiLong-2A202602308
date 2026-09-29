# Guideline patch

- **Rule mới đề xuất:** R01b — Vật cao ≥40 px nhưng rộng <20 px **và** không nhận ra được class (không thấy bánh
  xe/người rõ ràng) thì vẽ `ignore_region` reason `unreadable` thay vì box; nếu nhận ra class thì vẫn box theo R01.
  Ví dụ: 032280 L4 Bike h=43, rộng 19 px [179,1000,198,1044] — mình box, reference không box và đánh `unreadable`
  cho vật kế bên [206,999,228,1022]. Kèm R07b: `ego_body` chỉ vẽ ở vùng thân/tay lái/tay người lái xe gắn camera
  (với ADASIND là góc dưới trái), không bao giờ phủ lên phương tiện khác.
- **Áp dụng cho:** cả 6 class động khi vật ở xa (center/mid), `ignore_region` reason `unreadable` và `ego_body`.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** R01 chỉ đặt ngưỡng chiều cao, R06 liệt kê
  `unreadable` nhưng không nói khi nào vật đạt ngưỡng mà mờ được chuyển sang ignore → người và reference quyết định
  khác nhau ở cùng một cụm vật (finding r3_diag 032280 L4, E2_guideline_gap). R07 nói vẽ ego ở frame có thân xe
  nhưng không mô tả vùng ego trông như thế nào với camera gắn trên xe máy, dẫn tới box `Bike` trên tay lái ego ở cả
  4 frame của mình.
- **`rules_version` mới:** v1.0.0 → v1.1.0
- **Hiệu lực từ:** round `rework` của Day 11; các export trước giữ nhãn rules v1.0.0.
