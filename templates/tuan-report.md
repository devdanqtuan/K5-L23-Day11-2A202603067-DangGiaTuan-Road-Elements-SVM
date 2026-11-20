# Báo cáo cá nhân — Ngày 11
Họ tên: Đặng Gia Tuấn               Mã học viên: 2A202603067  
Phiên bản CVAT (Help → About): v2.82.0 (Server: v2.74.1)  
Guideline: Road v1.0                Lần nộp (`revision` trong manifest): 2  
Thời điểm export (ngày giờ trên CVAT): 2026-11-20T17:42:00Z    Số lần nộp lại sau Rework: 1  

## 1. Kiểm project
| Mục | Đúng như guideline Road v1.0? | Ghi chú |
| --- | --- | --- |
| 6 label, đúng kiểu | Đúng | lane: polyline; road_curb: polyline; crosswalk: polygon; drivable: polygon; traffic_light: rectangle; traffic_sign: rectangle |
| Thuộc tính và giá trị | Đúng | lane (position: L1/R1/L2/R2/other, style: solid/dashed, color: white/yellow/other, kind: single/double); drivable (area_type: direct/alternative); traffic_light (color: red/yellow/green/off/unknown, type: vehicle/pedestrian/beacon, facing: front/side/back, edge_zone: checkbox); traffic_sign (group: prohibitory/mandatory/warning/information/supplementary/other, facing: front/side/back, edge_zone: checkbox) |
| 4 tag ảnh | Đúng | camera (front, fisheye_front, fisheye_rear, fisheye_left, fisheye_right); timeofday (day, dusk, night); weather (clear, overcast, rain, fog, other); source (real, simulated). Mỗi tag có thuộc tính select tên 'value'. |
Cách kết thúc polyline trên bản CVAT này: Nhấn phím **N** trên bàn phím (hoặc nhấp đúp chuột vào điểm neo cuối cùng).  
Cách đặt tag ảnh trên bản CVAT này: Nhấn công cụ **Tag** (phím tắt T) ở thanh công cụ bên trái, chọn label tag tương ứng; sau đó mở rộng mục **Details** trong bảng Objects bên phải để chọn thuộc tính `value` chính xác.  

## 2. Số lượng đã gán
| Task | Ảnh | lane | road_curb | crosswalk | drivable | traffic_light | traffic_sign |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| front | 12 | 32 | 12 | 3 | 27 | 27 | 35 |
| front-extra (đã làm __ / 4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| fisheye | 8 | 9 | 12 | 8 | 11 | 3 | 3 |
| fisheye-extra (đã làm __ / 4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

## 3. Bảng ca khó
| Ảnh | Nhãn | Phân vân giữa | Đã chọn | Lý do |
| --- | --- | --- | --- | --- |
| A05 | `lane` | Cố vẽ ước lượng L1/R1 hay không vẽ | Không vẽ L1/R1, chỉ gán `road_curb` mép đường | Mặt đường bê tông cũ, bóng râm và cây che khuất hoàn toàn vệt sơn. Theo Checkpoints và Guideline 3: Không vẽ vạch mà ảnh không thấy được; đã xin Coach xác nhận miễn QC1-02. |
| A04 | `lane` (vạch vàng tim đường) | Chọn `style = solid` hay `dashed` | `kind = double`, `style = solid`, `color = yellow` | Vạch đôi một liền một đứt (QCVN 1.4). Road v1.0 chưa có giá trị hỗn hợp, ưu tiên chọn vạch liền màu vàng phía bên làn xe đang lưu thông. |
| B05 | `lane` (camera sườn trái MVL) | Chọn `position = L1` hay `other` | `position = other` | Theo Guideline Mục 4 và Rubric: Mọi vạch trên camera hai bên sườn (MVL/MVR) bắt buộc gán `position = other` để tránh lỗi nặng QC1-03. |
| B04 | `traffic_sign` & `traffic_light` | Bật hay tắt checkbox `edge_zone` | Bật `edge_zone = true` | Tâm hộp bounding box nằm cách tâm ảnh > 80% bán kính ảnh vòng cầu ($r > 512\text{ px}$ trên ảnh 1280x966). |

## 4. Phiếu QC 1 nhận được
Bạn kiểm lô của tôi: Lam Hoai Kim Ngan (`lamhoaikimngan`)    Quyết định nhận được: **Rework**  
*Ghi chú từ phiếu của bạn Ngân*: Lô revision 1 có 2 lỗi nặng (QC1-02 thiếu R1 trên A04, QC1-03 gán vạch bên phải thành L1 trên A04), 5 lỗi chính (7.6% > 5%) và 2 lỗi nhẹ. Đã chuyển trạng thái xử lý Buffer và nộp lại với manifest revision 2.

| Mã lỗi | Số lỗi | Đồng ý? | Đã sửa? |
| --- | ---: | --- | --- |
| QC1-02 | 1 | Đồng ý (Ảnh A04 bị gán nhầm vạch R1 thành L1 dẫn đến thiếu R1) | Đã sửa: Chuyển vạch bên phải về đúng `position = R1` |
| QC1-03 | 1 | Đồng ý (Ảnh A04 gán vạch bên phải xe chạy thành L1) | Đã sửa: Đổi position từ L1 thành R1 |
| QC1-04 | 1 | Đồng ý (Ảnh B06 có polyline lane#0 vẽ ngược chiều từ xa về gần) | Đã sửa: Đảo ngược chuỗi điểm neo để vẽ từ gần ra xa |
| QC1-05 | 2 | Đồng ý (Ảnh A04 có 2 polyline L1 vẽ lặp; A03 vạch R1 bị xe che nhưng chưa bật occluded) | Đã sửa: Xóa polyline vẽ lặp trên A04; bật `occluded = true` cho R1 trên A03 |
| QC1-06 | 1 | Đồng ý (Ảnh A04 gán nhầm màu vạch: vạch phải trắng gán vàng; tim đường vàng lại gán trắng) | Đã sửa: Vạch tim đường là yellow double solid; vạch bên phải là white dashed single |
| QC1-11 | 1 | Đồng ý (Ảnh B03 biển báo ngoài 80% bán kính ảnh fisheye chưa bật edge_zone) | Đã sửa: Bật checkbox `edge_zone = true` |
| QC1-12 | 2 | Đồng ý (Ảnh A03 và A04 có cặp điểm neo lặp liền nhau do click đúp) | Đã sửa: Đã làm sạch toàn bộ điểm lặp thừa |

## 5. Kiểm file export
| File | Mở lại được? | Số shape ảnh 1 (job / file) | Số shape ảnh 2 (job / file) | Tag camera có trong XML? |
| --- | --- | --- | --- | --- |
| `tuan-front.zip` | Có (lint PASS) | 8 / 8 | 6 / 6 | Có (`<attribute name="value">front</attribute>`) |
| `tuan-fisheye.zip` | Có (lint PASS) | 5 / 5 | 5 / 5 | Có (`<attribute name="value">fisheye_front</attribute>`) |

## 6. Ảnh chụp màn hình
1. **Tab Labels của project**: Ảnh chụp màn hình Project Settings -> Labels thể hiện đủ 6 nhãn hình học (lane, road_curb, crosswalk, drivable, traffic_light, traffic_sign) và 4 nhãn Tag (camera, timeofday, weather, source).
2. **Một ảnh camera trước đã gán đủ**: Ảnh chụp A01_bc5fadac-fa6dd225.jpg gán hoàn chỉnh polyline làn (L1, R1, L2, R2), đa giác drivable (direct, alternative) và rectangle đèn/biển báo.
3. **Một ảnh fisheye đã gán**: Ảnh chụp B06/B08 trên WoodScape phóng to cung cong đa điểm của vạch kẻ làn ôm sát viền mắt cá.
4. **Một đoạn XML có `<polyline label="lane">`**: Ảnh chụp màn hình file `annotations.xml` giải nén từ file zip thể hiện cấu trúc thẻ `<polyline label="lane" ...>` kèm các thẻ con `<attribute name="position">...`.

## 7. Tự đánh giá
* **Lỗi bạn mắc nhiều nhất hôm nay (theo mã QC1-xx), và cách tránh ở Ngày 20**:  
  Lỗi dễ mắc nhất là **QC1-05** (vạch đứt bị phân tách do thói quen vẽ ngắt quãng từng vệt sơn hoặc lặp nét) và **QC1-03 / QC1-06** (nhầm lẫn giữa L1/R1 và quy ước màu sắc tim đường vàng đôi). Cách tránh ở Ngày 20: Luôn xác định tâm làn xe đang đi làm mốc chuẩn trước khi gán (bên trái là L1, bên phải là R1); vạch tim đường phân chia hai chiều xe chạy luôn là màu vàng (yellow double); vạch đứt luôn vẽ bằng 1 polyline duy nhất kéo dài từ đầu đến cuối vệt sơn.
* **Nhãn của bạn đi vào model ở đâu? Chọn một lỗi đã sửa và nói nó làm model học sai thế nào**:  
  Dữ liệu nhãn polyline làn đường được dùng trực tiếp để huấn luyện mô hình phát hiện làn (Lane Detection / Row-anchor CNN) và hệ thống hỗ trợ giữ làn đường (Lane Keeping Assist - LKA). Nếu mắc lỗi **QC1-03 (gán nhầm vạch bên phải thành L1)**, mô hình AI sẽ nhầm lẫn nghiêm trọng về ranh giới làn đường của xe, tưởng rằng cả hai vạch đều nằm bên trái. Khi đưa vào thuật toán điều khiển LKA, xe tự hành sẽ tính toán sai hoàn toàn độ lệch tâm làn (lateral offset), dẫn đến việc bẻ lái đột ngột sang phải và lao ra khỏi mặt đường xe chạy.

## 8. (Tùy chọn, buffer) Từ polyline đến đích row-anchor
| y | 720 | 680 | 640 | 600 | 560 |
| --- | ---: | ---: | ---: | ---: | ---: |
| x của chúng tôi (tuan) | 712.6 | 656.0 | 599.4 | 542.2 | 484.5 |
| x của bạn kia (lamhoaikimngan) | không có làn | 657.6 | 601.3 | 544.3 | 487.4 |

### Trả lời 3 câu:
1. **Năm số x đó chính là đích mà một model row-anchor học cho vạch này. Nếu bạn dừng polyline sớm 40 px, hàng nào đổi thành "không có làn"?**  
   *Trả lời*: Hàng **$y = 720$** (hoặc hàng xa nhất nếu dừng sớm ở phía xa) sẽ đổi thành "không có làn" vì polyline không còn bao phủ tới tọa độ hàng quét đó (giống như kết quả của bạn Ngân ở hàng 720 do điểm đầu bắt đầu tại $y=718.08$).
2. **So với bạn được kiểm (cùng ảnh): ở hàng 720 và hàng 560, hai bạn lệch nhau bao nhiêu pixel? Lệch ở hàng nào ảnh hưởng LKA nhiều hơn (slide 25)?**  
   *Trả lời*:  
   - Ở hàng 720: Bạn Ngân không có làn (mất điểm neo gần xe), trong khi bài của Tuấn có $x=712.6\text{ px}$.  
   - Ở hàng 560: Lệch $|484.5 - 487.4| = 2.9\text{ px}$.  
   - **Lệch ở hàng 720 (gần xe)** ảnh hưởng đến thuật toán giữ làn LKA nhiều hơn rất nhiều, vì điểm neo sát xe quyết định trực tiếp sai số vị trí ngang (lateral deviation) tức thời của xe so với tim đường. Mất hoặc lệch điểm neo ở cự ly gần sẽ khiến bộ điều khiển đánh lái giật cục ngay lập tức.
3. **Nếu polyline vẽ từ xa về gần (ngược quy tắc 2), cách tính trên có đổi không? Script nào sẽ gặp khó?**  
   *Trả lời*:  
   - Về mặt toán học nội suy đoạn thẳng thuần túy giữa 2 điểm liên tiếp, giá trị $x$ có thể không đổi, nhưng thứ tự danh sách điểm trong file XML bị đảo ngược.  
   - Các script nội suy và huấn luyện mô hình Deep Learning (như row-anchor model, hoặc script `road_check` kiểm tra góc vector `s1 < s0`) sẽ báo lỗi nghiêm trọng hoặc gán nhãn điểm neo sai thứ tự, làm mô hình không thể hội tụ.


![Tab Labels của project trên CVAT](templates/labels_project_on_CVAT.png)

![Một ảnh front đã gán hoàn chỉnh](templates/Front_full.png)

![Một ảnh fisheye phóng to đường cung cong của polyline.](templates/Polyline_fisheye.png)

![Một đoạn code XML mở từ file zip có thẻ <polyline label="lane">](templates/polyline_lane.png)