# Báo cáo Day 13 — Nguyễn Văn Nhất, 2A202603278

## 1. Kết luận trước
- MSSV `2A202603278`, task `D13-2A202603278` (CVAT local, task #14).
- Môi trường Windows, CVAT `2.81.0`. Trình duyệt và MSSV bạn ngồi cạnh: TODO(học viên).
- Frame theo thứ tự: `s01f25`, `s01f49`, `s02f04`, `s02f24`, `s03f06`, `s03f25`, `s04f08`, `s04f51`, `s05f10`, `s05f29`.

Bản xuất gần nhất có **61 cuboid**, trong đó **5/61** có cờ tự kiểm. Năm frame có ít cuboid nhất, mỗi frame 5 hộp, là `s01f49`, `s02f04`, `s03f25`, `s04f51` và `s05f29`. TODO(học viên): bổ sung lý do số hộp ở các frame này thấp hơn và phần bạn tự tin nhất.

- Góc nhìn: kéo chuột trái trong khung 3D để xoay xuống góc nhìn từ trên, kéo chuột phải để dời khung và lăn con lăn để phóng to hoặc thu nhỏ.
- Vẽ cuboid: chọn nhãn và chế độ **Shape**, nháy đúp vào cụm điểm để đặt hộp, chỉnh tâm và góc xoay trong **Top**, rồi kiểm chiều cao và đáy trong **Side** và **Front**.
- Kiểm hướng: bật **Appearance → Cuboid orientation**, đối chiếu vạch đỏ với đầu xe trên ảnh camera. Sau khi **Save**, dùng `preview` (hoặc `sync`) để xem hình chiếu cuboid lên ảnh camera trong `work/xem/`.

## 2. Đọc frame (Bước 2)
Hai frame đọc kỹ và ảnh minh chứng:

- `s01f25`: `t1-s01f25-3d.png`, `t1-s01f25-top.png`.
- `s01f49`: `t1-s01f49-3d.png`, `t1-s01f49-top.png`.

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| `s01f25` | −y chéo lên trái, +x chéo xuống trái, +y chéo xuống phải. | Vùng quét gần có các vòng điểm mặt đường liên tục. Chưa đo độ dốc và cao độ trên ảnh. | Xe con #7 ở 25,41 m là vật xa nhất trong các cuboid đã vẽ ở frame này. | Xe con, người đi bộ. | Xe con #2 ở sau bên phải; hai cụm dáng người ở bên phải. | Cụm điểm rời về phía mép phải ảnh Top, chưa đủ chi tiết để định loại. |
| `s01f49` | −y chéo lên trái, +x chéo xuống trái theo hướng dốc hơn, +y chéo xuống phải. | Có hai dải điểm dọc hai mép vùng đường. Chưa đo độ dốc và cao độ trên ảnh. | Xe con #5 ở 16,22 m là vật xa nhất trong các cuboid đã vẽ ở frame này. | Xe con. | Xe con #1 phía sau, gần trục dọc của xe thu dữ liệu. | Cụm điểm rời ngoài mép đường ở vùng dưới ảnh Top, chưa đủ chi tiết để định loại. |

LiDAR cho biết khoảng cách theo mét đến các cụm đã gán nhãn, vị trí của chúng quanh xe thu dữ liệu và hình dạng mặt đường qua các dải điểm quét. TODO(học viên): bổ sung ba chi tiết cụ thể đã thấy khi đối chiếu ảnh camera của hai frame.

Tám frame lướt:

| Frame | Phía trước ở cạnh nào của ảnh nhìn từ trên xuống | Điều lạ (nếu có) |
|---|---|---|
| `s02f04` | ở cạnh -y | |
| `s02f24` |ở cạnh -y | |
| `s03f06` | ở cạnh -y  | |
| `s03f25` | ở cạnh -y | |
| `s04f08` | ở cạnh -y | |
| `s04f51` | ở cạnh -y | |
| `s05f10` | ở cạnh -y | |
| `s05f29` | ở cạnh -y | |

## 3. Ba cuboid có hướng dẫn (Bước 3)
Ảnh: `t2-a-car`, `t2-b-bus-truck`, `t2-c-ped-cyclist`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | `s01f25`, Car #2 | Car | Có vạch đỏ dọc thân hộp; cần camera để xác nhận đầu xe. | Đáy gần đường điểm trong Side/Front. | Ảnh có đủ ba khung, cần đối chiếu camera. | Đối chiếu sau khi xác nhận idx. |
| (b) xe buýt hoặc xe tải | `s02f04`: bus #18 | Bus theo màu nhãn | Vạch đỏ dọc chiều dài hộp; cần camera để xác nhận đầu xe. | Đáy gần dải điểm nền trong Front. | Ảnh có đủ ba khung, cần đối chiếu camera. | Đối chiếu sau khi xác nhận idx. |
| (c) người đi bộ hoặc xe hai bánh | `s01f25`, pedestrian #4 | Pedestrian | Có vạch đỏ ngang ảnh; hướng đi cần đối chiếu camera. | Các điểm quanh chân thưa; kiểm tra bề mặt dưới chân vì cả hai hộp có cờ đáy cao. | Ảnh có đủ ba khung, cần đối chiếu camera. | Hai hộp có kích thước 0,60 × 0,60 × 1,00 m. Chiều cao nằm trong khoảng tham chiếu trẻ em; ảnh LiDAR chưa đủ để xác nhận độ tuổi. |

không có : vòng hiệu chuẩn: cuboid của bạn lệch so với các bạn ở điểm nào, cả lớp chốt cách hiểu guideline nào? Không có.

## 4. Bốn hiện tượng chất lượng (Bước 4)
Nguyên nhân ghi là **giả thuyết**. Frame mượn thì ghi rõ nguồn: task chung, hoặc MSSV của bạn cho mượn.

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | `t4-1-bong-che.png` · `s05f10` truck#56 | Một khối xe lớn có thể chắn tia tới vùng phía sau. | Điểm tập trung rõ ở mặt gần của khối lớn, vùng sâu hơn ít điểm. | Đối chiếu camera để xác nhận che khuất; chỉ suy phần bị che khi còn đủ căn cứ. |
| 2. Thưa ở xa | `t4-2-thua-o-xa.png` · `s03f25` car#36 | Ở xa, khoảng cách giữa các tia quét trên bề mặt lớn hơn. | Chỉ còn vài hàng điểm rời trên mặt xe, biên dọc thân rất thưa. | Dựa vào camera và các biên còn đọc được để căn hộp; đo khoảng cách trước khi quyết định gán. |
| 3. Phản xạ thấp / ít điểm bất thường | `s03f25`, car#34 | Bề mặt hoặc góc tới có thể làm tín hiệu phản hồi yếu. | Một mặt có các hàng điểm rõ hơn, trong khi phần thân chỉ có ít điểm mờ và rời. | Đối chiếu camera; ghi LOW_VISIBILITY nếu chưa đủ bằng chứng để xác định biên. |
| 4. Điểm lạ | `t4-4-diem-la.png` · `s03f25` car#36 | Các điểm rời có thể do nhiễu hoặc tín hiệu phản xạ ngoài bề mặt đang xét. | Có một số điểm rời tách khỏi dải điểm chính; ảnh chưa xác nhận nguồn của từng điểm. | Kiểm tra xem cụm rời có hình dạng nhất quán trên nhiều góc nhìn trước khi dùng để căn biên. |

## 5. Số từ cuboid của tôi (Bước 5)
Lấy từ `submission/D13-<MÃ SỐ>-bangA.csv`; số điểm lấy từ cột `points_in` của `-tukiem.csv`. Mặt đường khoảng −1,7 m: z đáy lệch hơn 0,3 m là lơ lửng hoặc chìm.

| Cuboid (frame, idx, class) | z | h | z đáy = z − h/2 | dist_m | r = √(x²+y²+z²) | yaw_deg | Nhận xét (lơ lửng / chìm / chạm đường) |
|---|---|---|---|---|---|---|---|
| `s01f25` #2 Car | −0,965 m | 1,400 m | −1,665 m | 6,92 m | 6,99 m | 270,2° | Gần cao độ mặt đường tham chiếu. |
| `s01f25` #4 Pedestrian | −0,416 m | 1,000 m | −0,916 m | 12,56 m | 12,57 m | 156,8° | Tự kiểm báo đáy cao 0,71 m so với mặt đường ước tính tại chỗ. |
| `s01f25` #7 Car | −1,344 m | 1,800 m | −2,244 m | 25,41 m | 25,44 m | 90,8° | Thấp hơn mốc tham chiếu chung. Cần so với mặt đường cục bộ. |

- Gần nhất trong bản xuất là Car `s02f04` #3, `dist_m` 2,99 m và r 3,18 m. Xa nhất là Car `s03f25` #4, `dist_m` 99,02 m và r 99,02 m.
- Car `s01f25` #2 có kích thước 4,55 × 1,80 × 1,40 m, nằm trong dải sedan/hatchback. Pedestrian #4 có kích thước 0,60 × 0,60 × 1,00 m, dưới dải chiều cao người lớn. Cần đối chiếu ảnh và mặt đất để phân biệt hộp thấp với người có vóc dáng nhỏ.
- Trong `s01f25`, Car #3 nằm phía trước và có yaw 267,0°, gần 270°. Car #7 có yaw 90,8°, gần hướng ngược chiều 90°.

Mật độ theo khoảng cách (4 cuboid ở các `dist_m` khác nhau):

| Cuboid (frame, idx, class) | dist_m | Số điểm (`points_in`) |
|---|---|---|
| `s01f25` #2 Car | 6,92 m | 1.783 |
| `s01f25` #8 Car | 12,53 m | 1.221 |
| `s01f25` #6 Car | 17,23 m | 818 |
| `s01f25` #7 Car | 25,41 m | 395 |

Trong bốn xe được chọn, xe ở 25,41 m xa gấp khoảng 3,7 lần xe ở 6,92 m và còn khoảng 22% số điểm. Số điểm giảm theo khoảng cách, cùng xu hướng với slide. Các mẫu riêng có thể khác mức trung bình do góc quan sát, che khuất và bề mặt phản xạ.

Sau khi chỉnh hai hộp Pedestrian #4 và #5 ở `s01f25`, khoảng hở dưới đáy theo tự kiểm giảm từ 0,76 m xuống 0,71 m với #4 và từ 0,82 m xuống 0,71 m với #5. Cả hai hộp vẫn có cờ đáy cao.


## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc
Đơn vị mét giúp mô hình học vị trí và kích thước thật trong không gian 3D, dù kích thước biểu kiến của vật thay đổi giữa các camera. Với vật thưa điểm, quy tắc về mức quan sát tối thiểu giúp tránh đưa các hộp có biên suy đoán quá nhiều vào dữ liệu huấn luyện.

- Công cụ AI: ChatGPT (Codex).
- Câu hỏi chính: thứ tự làm lab, cách thao tác CVAT và cách chạy các lệnh xuất bài.
- Cách dùng kết quả: tham khảo thao tác CVAT, soạn bản nháp báo cáo.

Điều còn chưa chắc là cao độ bề mặt ngay dưới chân hai người ở `s01f25`. Phiếu tự kiểm vẫn báo đáy hai hộp cao hơn mặt đường ước tính, nên cần đối chiếu điểm tại chỗ trong Side và Front để xác định đáy hộp.
