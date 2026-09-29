# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `aa43f1c0f7ca471bf3b512b0ca67294701670d4572faba225d5b8eb9e0348a54`; slice `B1-mid`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_014670.jpg, adasind_032280.jpg, adasind_034080.jpg. Frame thiếu trong export: không.
TP=17; FP=9; FN=3; số lần đối chiếu=27; mean IoU của TP=0.832.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.630 | 0.926 | 0.852 |
| precision | 0.654 | 0.484 | 0.000 |
| recall | 0.850 | 0.600 | 0.000 |
| jaccard | 0.586 | 0.456 | 0.000 |
| dice | 0.739 | 0.532 | 0.000 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 4 | 2 | 0 | 0.926 | 0.667 | 1.000 | 0.667 | 0.800 |
| Bus | 0 | 1 | 0 | 0.963 | 0.000 | 0.000 | 0.000 | 0.000 |
| Car | 4 | 3 | 1 | 0.852 | 0.571 | 0.800 | 0.500 | 0.667 |
| Pedestrian | 5 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| ThreeWheeler | 4 | 2 | 1 | 0.889 | 0.667 | 0.800 | 0.571 | 0.727 |
| Truck | 0 | 1 | 1 | 0.926 | 0.000 | 0.000 | 0.000 | 0.000 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_014670.jpg | 4 | 2 | 1 | 0.667 | 0.667 | 0.800 |
| adasind_032280.jpg | 6 | 2 | 0 | 0.750 | 0.750 | 1.000 |
| adasind_034080.jpg | 7 | 5 | 2 | 0.538 | 0.583 | 0.778 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Bus | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|---:|
| Bike | 4 | 0 | 0 | 0 | 0 | 0 | 0 |
| Bus | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Car | 0 | 0 | 4 | 0 | 0 | 0 | 1 |
| Pedestrian | 0 | 0 | 0 | 5 | 0 | 0 | 0 |
| ThreeWheeler | 0 | 0 | 0 | 0 | 4 | 1 | 0 |
| Truck | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| <extra> | 2 | 0 | 3 | 0 | 2 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
