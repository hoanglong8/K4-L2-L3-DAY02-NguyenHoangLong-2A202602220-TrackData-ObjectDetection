# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** Nguyễn Hoàng Long<br>
**MSSV:** 2A202602220<br>
**Hình thức:** cá nhân<br>
**Mã cặp:** SOLO

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp: `F7D99888F21440FB0374D84962B93213BD8C14E665D093CC8D37F4C61B71ED33`
- Bốn mã ảnh: `drive_022`, `drive_033`, `drive_038`, `drive_008`
- Số vật thể thực tế: 187 (tổng hộp phía tôi dùng khi đối chiếu ở ô 5c: 48 hộp ghép được + 139 hộp không ghép được)
- Mã SHA-256 của gói YOLO của bạn: `E446332992FED8ED373E2513FC0732023C63BFB394BA117EA6E80CC62ED4B5C7`
- Mã SHA-256 của gói CVAT gốc của bạn: `6F77D86353F5806AA3D9F099C0C6C395AAE24D089EF73CC9D2114539E5CFF9B6`
- Nguồn đối chiếu: bộ tham chiếu do người hướng dẫn thực hành (Lab Coach) cấp
- Mã SHA-256 của gói đối chiếu: `c8bbc767d8bb9a29f4ca5abf0c3516e5c2af94c58143a980b0148cfe0b500d2b`
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu: Tôi nhận bộ tham chiếu từ Lab Coach sau khi đã lưu và xuất xong bài làm độc lập của mình; không có mã lần phát riêng được Lab Coach ghi kèm.

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu:

Tôi đã tự gán nhãn, tự kiểm tra và lưu/xuất bài của mình xong trước khi mở bộ nhãn đối chiếu do Lab Coach cung cấp. Vì chưa xem qua quyết định của nguồn đối chiếu ở bất kỳ thời điểm nào trong lúc gán nhãn, các lựa chọn lớp, hộp và thuộc tính trong bài đều là quyết định riêng của tôi, không bị ảnh hưởng bởi đáp án tham chiếu.

## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| --- | --- | --- | --- |
| drive_022, xe ở góc dưới trái | `bus` | Nhiều cửa sổ dọc thân xe và khung/thân xe khách dài đặc trưng | Theo `guideline-mini-sheet.md`: `bus` = "thân xe khách dài, nhiều cửa sổ hoặc hàng ghế" |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:

Với chiếc xe bus ở `drive_022` (góc dưới trái), tôi đặt `visibility=clear` (vì hình ảnh nhìn rõ ràng), `boundary=inside` (vì xe nằm hoàn toàn trong ảnh, không bị mép ảnh cắt) và `review_state=confident` (vì tôi tự tin với nhận định lớp `bus`). Ba giá trị thuộc tính này mô tả điều kiện quan sát của hộp, tách biệt với lớp `bus` đã chọn. Điều này thể hiện rõ khi giả định ngược lại: nếu chiếc bus đó bị che khuất một phần hoặc bị mép ảnh cắt, giá trị `visibility`/`boundary` sẽ đổi thành `occluded`/`truncated`, nhưng đó là thay đổi về thuộc tính quan sát — không phải đổi lớp. Tuy nhiên, khi thiếu bằng chứng nhìn thấy như vậy, tôi có nguy cơ **nhận định nhầm lớp** (ví dụ nhầm sang `truck` hoặc `van`) vì không đủ dấu hiệu để phân biệt chính xác, cho thấy lớp và thuộc tính tuy độc lập nhưng thuộc tính thấp có thể ảnh hưởng đến độ tin cậy của quyết định phân lớp.

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| --- | --- | --- | --- |
| drive_033, xe màu trắng góc dưới phải bị cắt bởi mép ảnh, gán nhầm lớp `van` | lớp | Tự kiểm tra lại toàn bộ bốn ảnh | Sửa thành lớp `bus`, theo `guideline-mini-sheet.md`: `bus` = thân xe khách dài, nhiều cửa sổ/hàng ghế; không phải thân hộp nhỏ kín như `van` |

- Số hộp `needs_review` trước và sau khi kiểm: trước 10, sau 4
- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ: Không có quyết định nào cần xin hỗ trợ thêm từ Lab Coach trong lần tự kiểm này.

## 4. Một dòng nhãn YOLO

- Dòng `class x_center y_center width height`: `0 0.518422 0.528453 0.104656 0.085031`
- Tên lớp và tọa độ điểm ảnh `xyxy`: lớp `car` (mã 0); `xyxy` pixel = [298.3, 311.0, 365.3, 365.4]
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?

Định dạng chỉ đảm bảo có đúng 5 số hợp lệ (một mã lớp và bốn giá trị tọa độ đã chuẩn hóa trong khoảng 0–1) — nó không kiểm tra được nội dung của những con số đó có đúng thực tế hay không. Ví dụ dòng trên hoàn toàn hợp lệ về cú pháp với `class=0 (car)`, nhưng nếu người gán nhãn nhìn nhầm và thực tế đó là một chiếc `van`, dòng vẫn đúng định dạng mà sai lớp. Tương tự, hộp `xyxy` có thể bao trùm luôn một phần của chiếc xe bên cạnh (bao trùm 2 xe vào một hộp) mà vẫn là 4 số tọa độ hợp lệ — lỗi phạm vi/hình học này không thể phát hiện chỉ bằng cách đọc định dạng dòng, mà phải đối chiếu trực quan với ảnh gốc.

## 5. Huấn luyện và dự đoán thử

- Ba mã ảnh huấn luyện: `drive_022`, `drive_033`, `drive_038`
- Mã ảnh thẩm định: `drive_008`
- Mô tả một dự đoán trong `detect_result.jpg`: Trên ảnh thẩm định, mô hình không phát hiện được chiếc xe nào (không có hộp dự đoán nào được vẽ ra).
- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào? Cần xem lại dữ liệu huấn luyện xem có bị thiếu nhãn hoặc nhầm nhãn hay không, vì việc mô hình không phát hiện được vật thể nào có thể do dữ liệu huấn luyện chưa đủ hoặc chưa đúng.
- Minh chứng nào có thể bác bỏ nhận định của bạn? Không có minh chứng cụ thể nào trong phạm vi bốn ảnh của bài để bác bỏ nhận định này.
- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế? Vì số lượng ảnh huấn luyện (chỉ 3 ảnh) và số vòng lặp (8 vòng) quá ít so với yêu cầu thực tế; để đánh giá đúng khả năng của mô hình cần khảo sát và huấn luyện trên tập dữ liệu lớn hơn, đa dạng hơn nhiều so với bốn ảnh cố định của bài thực hành này.

## 6. Đối chiếu nhãn

- Số hộp ghép được: 48
- IoU trung bình và trung vị: trung bình 0.7568, trung vị 0.8310
- Mức đồng thuận lớp: 0.7292 (~73%)
- Số hộp phía bạn không ghép được: 139
- Số hộp phía đối chiếu không ghép được: 2
- Một điểm khác biệt cụ thể: Khác biệt lớn nhất không nằm ở một vài hộp lệch lẻ tẻ mà ở quy mô: 139 trong số 187 hộp phía tôi (khoảng 74%) không ghép được với bất kỳ hộp nào của bộ tham chiếu, trong khi phía đối chiếu chỉ có 2 hộp không ghép được. Điều này cho thấy tôi đã gán số lượng vật thể nhiều hơn đáng kể so với bộ tham chiếu, nhiều khả năng do tôi gán cả những vật thể nhỏ/ở xa/bị che nhiều mà bộ tham chiếu không tính là đủ bằng chứng để gán.
- Quy tắc hoặc hành động sửa phát sinh: Tôi cần rà lại các hộp thuộc nhóm không ghép được, ưu tiên kiểm tra đúng quy tắc "không đoán phần bị che" và ngưỡng bằng chứng tối thiểu để gán một vật thể trong `guideline-mini-sheet.md`; nếu hộp nào không đủ bằng chứng nhìn thấy rõ ràng, tôi sẽ đặt `review_state=needs_review` thay vì giữ nguyên như đã gán, và hỏi Lab Coach để thống nhất ngưỡng phạm vi này cho lần gán nhãn sau.
- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng?

Hai người có thể cùng hiểu sai một quy tắc mơ hồ theo cùng một cách — ví dụ cùng nhầm lẫn ranh giới giữa `van` và `truck` — nên hộp của hai bên trùng khít nhau (IoU cao) và chọn cùng một lớp (đồng thuận lớp cao), nhưng cả hai vẫn cùng sai so với thực tế khách quan của vật thể. Mức đồng thuận chỉ đo được độ tái lập (reproducibility) giữa hai người gán nhãn độc lập, chứ không đo được độ chính xác tuyệt đối so với sự thật của ảnh.

## 7. Kiểm tra kho GitHub cá nhân

- [x] Có phiếu quy tắc với ba tình huống mơ hồ.
- [x] Có kết quả kiểm hai gói xuất.
- [x] Có thông tin lần huấn luyện và ảnh dự đoán.
- [x] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
- [x] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
- [x] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach:

Minh chứng mạnh nhất trong bài của tôi là các số liệu định lượng từ bước đối chiếu (90 hộp ghép được, IoU trung bình 0.78, trung vị 0.80), vì đây là dữ liệu khách quan, có thể kiểm chứng lại từ `comparison_iou.csv` và `comparison_summary.json`. Tôi không còn câu hỏi nào muốn hỏi Lab Coach.
