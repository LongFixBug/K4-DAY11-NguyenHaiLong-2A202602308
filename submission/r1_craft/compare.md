# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_014670.jpg
- L1 edge IGNORE_SCOPE
- L4+R5 mid WRONG_CLASS
- L6 center SPURIOUS
## adasind_032280.jpg
- L2 edge IGNORE_SCOPE
- L1+R1 mid ATTRIBUTE
- L4 mid SPURIOUS
- L6 edge SPURIOUS
## adasind_034080.jpg
- L12 edge IGNORE_SCOPE
- L10+R3 edge ATTRIBUTE
- L2+R9 center WRONG_CLASS
- L4 mid SPURIOUS
- L6+R2 center BOX_GEOMETRY
- L8 center SPURIOUS
- L11 center SPURIOUS

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 7 | 5 | 2 | 5 |
| mid | 11 | 10 | 1 | 3 |
| edge | 2 | 2 | 0 | 1 |
