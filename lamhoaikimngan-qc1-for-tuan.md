# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên): tuan        Người kiểm (học viên): Lam Hoai Kim Ngan (lamhoaikimngan)  
Ngày: 2026-11-21                      Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): tuan-front.zip, tuan-fisheye.zip (`revision` 1)  
Số ảnh kiểm: 6 (+ 0 ảnh thêm, ghi riêng)   Số nhãn đã kiểm (shape + tag): 66  

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 24 | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 6 | 1 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 8 | 1 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 8 | 1 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 8 | 2 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 8 | 1 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 16 | 0 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 2 | 0 | nặng / chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 7 | 0 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 5 | 0 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 20 | 1 | chính |
| QC1-12 file | lỗi file | 66 | 2 | nhẹ / nặng |

## Danh sách lỗi (mỗi lỗi một dòng)
| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-02 | A04_b7a8e795-0ff55db2.jpg | lane (R1) | nặng | Thiếu vạch R1 khi thấy rõ trên ảnh (bị gán nhầm sang L1) |
| 2 | QC1-03 | A04_b7a8e795-0ff55db2.jpg | lane#7@(719,536) | nặng | Sai position: vạch bên phải xe chạy nhưng gán position = L1 thay vì R1 |
| 3 | QC1-05 | A04_b7a8e795-0ff55db2.jpg | lane (3 polylines) | chính | 3 polyline cùng mang position = L1 (trùng position và tách vạch) |
| 4 | QC1-06 | A04_b7a8e795-0ff55db2.jpg | lane#7@(719,536) & lane#4@(27,553) | chính | Sai color/kind: vạch bên phải màu trắng lại gán yellow; vạch tim đường vàng đôi lại gán white |
| 5 | QC1-05 | A03_b1c66a42-6f7d68ca.jpg | lane#3@(881,659) | chính | Vạch R1 có đoạn bị che khuất nhưng không bật cờ occluded |
| 6 | QC1-11 | B03_04267_RV.png | traffic_sign#3@(1132,362) | chính | Quên bật edge_zone (tâm box ngoài 80% bán kính ảnh: dist ≈ 513 px > 512 px) |
| 7 | QC1-04 | B06_04068_MVL.png | lane#0@(921,496) | chính | Vẽ ngược chiều trên fisheye trái (điểm đầu y=496, điểm cuối y=784 xa về gần xe) |
| 8 | QC1-12 | A04_b7a8e795-0ff55db2.jpg | drivable#0@(89,549) | nhẹ | Có 1 cặp điểm lặp liền nhau (click đúp tạo điểm thừa) |
| 9 | QC1-12 | A03_b1c66a42-6f7d68ca.jpg | lane#3@(881,659) | nhẹ | Có 1 cặp điểm lặp liền nhau tại (611.2, 398.54) |

## Ca mơ hồ
| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| A03_b1c66a42-6f7d68ca.jpg | Lề trái của ảnh A03 là vỉa hè được gán bằng road_curb, không có vạch sơn kẻ đường L1. Trường hợp này có được miễn QC1-02 theo quy định không? | Được miễn theo CHECKPOINTS (ảnh không có vạch sơn thì không vẽ thêm, đã có coach xác nhận). |

## Quyết định
Lỗi nặng: 2   Lỗi chính: 5 / 66 nhãn = 7.6 %   Lỗi nhẹ: 2   Ca chờ lead: 0  
Quyết định (Release / Hold / Rework): **Rework**  
Lý do (theo luật của phiếu): Có 2 lỗi nặng (QC1-02 thiếu R1 trên A04, QC1-03 gán vạch bên phải xe thành L1 trên A04); đồng thời tỉ lệ lỗi chính 5/66 = 7.6% vượt ngưỡng 5% quy định → Rework.

Loại lỗi: bỏ sót = thiếu nhãn phải có; thừa = gán cái không phải đối tượng; sai phạm vi = hình lệch, sai chiều, dừng sai chỗ, tràn vùng. Ba loại này theo cách chia lỗi gán nhãn của ISO/IEC 5259-4 (mục 8.4.2); "sai thuộc tính" và "lỗi file" là khóa học thêm. Cách xếp mức: [thang mức lỗi](docs/qc_severity_road_v1.0.md).
