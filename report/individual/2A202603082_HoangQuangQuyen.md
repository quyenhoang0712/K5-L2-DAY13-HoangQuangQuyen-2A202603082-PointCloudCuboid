# Báo cáo Day 13 — Hoàng Quảng Quyền, 2A202603082

> **Cách dùng mẫu này:** đổi tên file thành `2A202603082_<HoTen>.md` (họ tên không dấu, viết liền), ví dụ `2A202600123_NguyenVanAn.md`. Đổi tên, đừng chép thành file thứ hai.
>
> Xoá mọi dấu TODO và dòng gợi ý (dòng bắt đầu bằng `>`). Mỗi con số lấy từ **bảng A** hoặc **phiếu tự kiểm** trong `submission/`, kèm đơn vị (m, °). Ảnh để trong `report/individual/images/` (nộp LMS trong file zip, không commit); trong báo cáo chỉ ghi **tên file ảnh**. Báo cáo này nằm trên repo public: không chép biển số, chữ trên biển hiệu hay biển chỉ đường, tên đường, địa danh; nhắc tới bạn khác thì ghi MSSV, không ghi họ tên. Cả báo cáo ~2 trang.

## 1. Kết luận trước

- MSSV: 2A202603082.
- Task: `D13-2A202603082`.
- 10 frame: s01f03, s01f53, s02f29, s02f54, s03f01, s03f48, s04f09, s04f44, s05f03, s05f33.
- Hệ điều hành: macOS.
- Trình duyệt: Google Chrome.
- Phiên bản CVAT: 2.82.1.
- MSSV bạn ngồi cạnh: 2A202603092.

Tổng cộng 66 cuboid trên 10 frame, trong đó frame s05f33 có ít nhất với 4 cuboid do [điền lý do thực tế]. Kết quả `selfcheck` có 0/66 cuboid bị cảnh báo. Phần tự tin nhất là car, phần còn hạn chế là người đi xe máy và đi bộ.

**Cách sử dụng CVAT:**

- Đổi góc nhìn: kéo chuột trái để xoay, chuột phải để di chuyển và dùng con lăn để phóng to, thu nhỏ hoặc quan sát từ trên xuống.
- Vẽ cuboid: chọn Shape, nháy đúp để đặt hộp và điều chỉnh qua Top, Side, Front.
- Bật Cuboid orientation để kiểm tra hướng vật thể và chạy `python scripts/lab.py preview` để đối chiếu cuboid trên ảnh camera.

## 2. Đọc frame (Bước 2)
Hai frame đọc kỹ. Ảnh: `t1-<frame>-3d`, `t1-<frame>-top` (ảnh nhìn từ trên xuống, có đánh dấu phía trước, +x, +y).

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| s01f03 | Phía trước (-Y) hướng lên trên bên trái, +X hướng xuống dưới bên trái, +Y hướng xuống dưới bên phải | Mặt đường khá phẳng, thấp hơn LiDAR khoảng 1,7 m | Khoảng 20 m | Xe con, xe buýt, xe tải và xe máy | Hai bên có dải phân cách | Khó xác định chiều dài xe do phần đuôi không rõ |
| s01f53 | Phía trước (-Y) nằm ở góc trên bên trái, +X hướng xuống trái, +Y hướng xuống phải | Đường tương đối bằng phẳng, thấp hơn LiDAR khoảng 1,7 m | Khoảng 20 m | Xe con, xe tải, xe buýt và xe máy | Có dải phân cách hai bên đường | Chưa xác định được hết phần thân sau để căn chiều dài xe |


Tám frame lướt:

| Frame | Phía trước ở cạnh nào của ảnh nhìn từ trên xuống | Điều lạ (nếu có) |
|---|---|---|
| s02f29 | Phía trên bên trái (-Y) | |
| s02f54 | Phía trên bên trái (-Y) | |
| s03f01 | Phía trên bên trái (-Y) | |
| s03f48 | Phía trên bên trái (-Y) | |
| s04f09 | Phía trên bên trái (-Y) | |
| s04f44 | Phía trên bên trái (-Y) | |
| s05f03 | Phía trên bên trái (-Y) | |
| s05f33 | Phía trên bên trái (-Y) | |

## 3. Ba cuboid có hướng dẫn (Bước 3)
Ảnh: `t2-a-car`, `t2-b-bus-truck`, `t2-c-ped-cyclist`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | s01f03, 2 | Car | 267.1°, cần đối chiếu hướng xe | Đáy -1.646 m, gần mặt đường | Cần xác nhận | Dài 5.196 m, rộng 1.780 m, cao 1.455 m; dài hơn khoảng tham chiếu sedan |
| (b) xe tải | s01f03, 9 | Truck | 89.5°, cần đối chiếu hướng xe | Đáy -1.555 m, gần mặt đường | Cần xác nhận | Nằm trong khoảng tham chiếu xe tải nhẹ |
| (c) xe hai bánh | s03f48, 5 | Cyclist | 272°, cần đối chiếu hướng xe | Đáy -1.323 m, cần kiểm tra lại | Cần xác nhận | Dài 1.205 m, rộng 1.043 m, cao 1.076 m; ngoài khoảng tham chiếu |


### 4. Bốn hiện tượng chất lượng (Bước 4)

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | s03f01 / t4-1-bong-che.png / Task cá nhân | Vật thể phía trước có thể chắn tia LiDAR | Vùng phía sau vật cản thiếu điểm | Bật occluded và chỉ suy luận phần bị che khi đủ cơ sở |
| 2. Thưa ở xa | s04f44 / t4-2-thua-o-xa.png / Task cá nhân | Khoảng cách xa làm giảm mật độ điểm | Vật thể có ít điểm, khó xác định đầy đủ hình dạng | Áp dụng quy tắc 15–20%, nếu không đủ cơ sở thì ghi LOW_VISIBILITY |
| 3. Phản xạ thấp / ít điểm bất thường | s05f03 / t4-3-phan-xa-thap.png / Task cá nhân | Bề mặt vật thể có thể phản xạ laser yếu | Số điểm thu được ít bất thường | Đối chiếu camera và tránh suy đoán kích thước khi thiếu căn cứ |
| 4. Điểm lạ | s02f35 / t4-4-diem-la.png / Task chung D13-HC485087 | Có thể do nhiễu cảm biến hoặc phản xạ bất thường | Cụm điểm xuất hiện bên dưới mặt đường | Không gán nhãn cho cụm điểm ngoại lai |

## 5. Số từ cuboid của tôi (Bước 5)

| Cuboid (frame, idx, class) | z (m) | h (m) | z đáy (m) | dist_m (m) | r (m) | yaw_deg (°) | Nhận xét |
|---|---:|---:|---:|---:|---:|---:|---|
| s01f03, 2, Car | -0.918 | 1.455 | -1.646 | 8.23 | 8.28 | 267.1 | Gần mặt đường |
| s01f03, 9, Truck | -0.335 | 2.439 | -1.555 | 19.01 | 19.02 | 89.5 | Gần mặt đường |
| s03f48, 5, Cyclist | -0.785 | 1.076 | -1.323 | 8.80 | 8.84 | 272.0 | Có dấu hiệu lơ lửng |


Mật độ theo khoảng cách (4 cuboid ở các `dist_m` khác nhau):

| Cuboid (frame, idx, class) | dist_m (m) | Số điểm (points_in) |
|---|---:|---:|
| s01f03, 1, Car | 9.2 | 1177 |
| s01f03, 2, Car | 8.2 | 1773 |
| s01f03, 3, Car | 15.7 | 541 |
| s01f03, 4, Car | 20.8 | 378 |

### Kiểm chéo

- Người kiểm chéo: Hồ Đăng Mạnh (2A202603092).
- Thông số đối chiếu: dist_m và yaw_deg của Car tại frame s01f03, idx 2.
- Kết quả: Cả hai thông số đều khớp với bảng A.
- Không cần chỉnh sửa sau kiểm chéo.


## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc

### (a) Ý nghĩa của nhãn

- Nhãn 3D đo bằng mét giúp mô hình học được kích thước, vị trí và khoảng cách thực tế của vật thể trong không gian.
- [Tự giải thích vì sao vật thưa điểm cần quy tắc riêng.]

### (b) Cách sử dụng AI

- Công cụ: ChatGPT.
- Nội dung hỗ trợ: Tìm hiểu guideline, xử lý lỗi CVAT, các lệnh Terminal, tính toán số liệu và trình bày báo cáo.
- Cách sử dụng kết quả: Tham khảo hướng dẫn, hỗ trợ đánh dấu trục tọa độ và đối chiếu thông tin trong quá trình thực hiện bài lab. Cuboid được gán nhãn thủ công.

### (c) Điều còn chưa chắc

- [Ghi những khó khăn thực tế còn gặp khi xác định kích thước, hướng hoặc phần bị che khuất của vật thể.]
