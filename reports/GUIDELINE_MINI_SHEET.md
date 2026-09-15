# Phiếu quy tắc gán nhãn — Ngày 2

**Họ và tên:** Nguyễn Hoàng Long<br>
**MSSV:** 2A202602220<br>
**Hình thức:** cá nhân<br>
**Mã cặp:** SOLO

## 1. Phạm vi

- Chỉ gán phương tiện thuộc bốn lớp bên dưới.
- Mỗi phương tiện là một hộp; không gộp nhiều xe.
- Không gán người, xe máy, xe đạp, biển báo hoặc phần phản chiếu.
- Vật thể quá nhỏ hoặc mờ đến mức không thể phân lớp có căn cứ: không đoán; ghi lý do vào nhật ký quyết định.

## 2. Bốn lớp cố định

| Mã | Lớp | Gán khi nhìn thấy | Không gán vào lớp này |
| ---: | --- | --- | --- |
| 0 | `car` (ô tô con) | sedan, hatchback, SUV, taxi, xe bán tải dùng như xe con | xe có thùng/ben rõ; thân xe buýt; xe van thân hộp |
| 1 | `truck` (xe tải) | thùng, ben, sàn hàng hoặc thiết bị công vụ rõ ràng | ô tô con; thân xe buýt; xe van kín một khối |
| 2 | `bus` (xe buýt) | thân xe khách dài, nhiều cửa sổ hoặc hàng ghế | xe van nhỏ; xe tải; ô tô con |
| 3 | `van` (xe van) | thân hộp nhỏ, kín, dùng chở người hoặc hàng | thân xe buýt; khoang hàng tách biệt như xe tải |

Thứ tự lớp là cố định: `0 car, 1 truck, 2 bus, 3 van`.

## 3. Hộp giới hạn

- Vẽ sát phần vật thể nhìn thấy.
- Không ước lượng phần bị xe khác che.
- Vật thể chạm mép ảnh vẫn được gán nếu đủ bằng chứng phân lớp.
- Không để hộp chứa nhiều nền hoặc nhiều phương tiện.

## 4. Ba thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
| --- | --- | --- |
| `visibility` (mức nhìn thấy) | `clear` (rõ), `occluded` (bị che), `unclear` (không rõ) | mức bằng chứng nhìn thấy |
| `boundary` (quan hệ mép ảnh) | `inside` (trong ảnh), `truncated` (bị cắt) | vật thể có bị mép ảnh cắt hay không |
| `review_state` (trạng thái xem lại) | `confident` (tự tin), `needs_review` (cần xem lại) | đánh dấu quyết định cần quay lại |

YOLO không lưu ba thuộc tính này. Vì vậy phải xuất thêm `CVAT for images 1.1` từ cùng công việc.

## 5. Ba tình huống mơ hồ

Hoàn thành trước khi xem bài của người khác hoặc bộ nhãn tham chiếu.

### Tình huống A — xe buýt hay xe van?

- Ảnh và mã vật thể: drive_033, xe màu trắng góc dưới phải
- Dấu hiệu nhìn thấy: Thân xe không thấy rõ nhiều cửa sổ ngay từ đầu nên dễ nhầm là van, nhưng khi phóng to thấy khung thân dài và nhiều cửa sổ dọc thân xe đặc trưng của xe khách
- Quy tắc áp dụng: Theo guideline-mini-sheet.md: bus = thân xe khách dài, nhiều cửa sổ/hàng ghế; van = thân hộp nhỏ, kín, không có thân xe buýt
- Quyết định: Sửa từ van thành bus
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Phóng ảnh lên 100% để quan sát kỹ số cửa sổ và khung xe; nếu vẫn không chắc, đặt review_state=needs_review và hỏi Lab Coach

### Tình huống B — xe tải hay xe van/ô tô con?

- Ảnh và mã vật thể: drive_033, xe car #14
- Dấu hiệu nhìn thấy: Thân xe không có thùng/ben và không có khoang hàng tách biệt hay thân hộp kín; ban đầu dễ nhầm là van do kích thước nhỏ, nhưng quan sát kỹ thấy đây là thân xe con thông thường
- Quy tắc áp dụng: Theo guideline-mini-sheet.md: car = sedan, hatchback, SUV, taxi, xe bán tải dùng như xe con; van = thân hộp nhỏ, kín, dùng chở người/hàng
- Quyết định: Sửa từ van thành car
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Phóng ảnh lên 100% để quan sát hình dáng thân xe; nếu vẫn không chắc, đặt review_state=needs_review và hỏi Lab Coach

### Tình huống C — bị che, bị mép ảnh cắt hay không đủ bằng chứng?

- Ảnh và mã vật thể: drive_008, xe car #92
- Dấu hiệu nhìn thấy khi phóng 100%: Xe nhìn rõ ràng, không bị che khuất, nhưng phần thân xe bị mép ảnh cắt mất một phần nên ban đầu dễ nhầm là van do không thấy trọn hình dáng thân xe
- Giá trị `visibility`: clear
- Giá trị `boundary`: truncated
- Trạng thái `review_state`: confident
- Lý do: Dù bị cắt mép ảnh, phần thân xe còn lại đủ rõ để nhận diện đặc điểm của car (không phải thân hộp kín của van), nên sửa nhãn thành car theo quy tắc định vật thể chạm mép ảnh vẫn được gán nếu đủ bằng chứng phân lớp trong guideline-mini-sheet.md

## 6. Xác nhận tự kiểm tra

- [ ] Đã rà đủ bốn ảnh.
- [ ] Đã kiểm vật thể thiếu và trùng.
- [ ] Đã kiểm lớp và hình học từng hộp.
- [ ] Mỗi hộp có đủ ba thuộc tính.
- [ ] Đã xử lý mọi hộp `needs_review`.
- [ ] Đã hoàn thành ba tình huống trước khi xem nguồn đối chiếu.
- [ ] Nếu làm theo cặp, hai người đã xuất bài độc lập trước khi trao đổi.
- [ ] Nếu làm cá nhân, bài riêng đã được kiểm trước khi nhận bộ tham chiếu.
- [x] Số vật thể thực tế: 187 - 40-60 là mục tiêu khối lượng, không phải điểm cắt.
