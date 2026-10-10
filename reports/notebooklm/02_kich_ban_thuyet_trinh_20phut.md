# Kịch bản độc thoại — Video thuyết trình 1 người (~20 phút)
### Đề tài: Ứng dụng AI vào Quản trị Vận hành — Process Mining

> **Đây là lời thoại độc thoại cho MỘT người trình bày** (ví dụ nộp cho giáo viên).
> Câu được viết ngắn, dễ đọc thành tiếng. Dấu `[ngắt]` là chỗ nghỉ ngắn khi đọc.
> Dòng *(Slide: …)* chỉ cho biết đang chiếu slide/hình nào — **không đọc** phần trong ngoặc.
> Xưng hô: **"em"** (một người nói với thầy/cô). Có thể dùng để tự đọc, lồng tiếng (text-to-speech), hoặc tạo video người ảo (AI avatar).

---

## Mở đầu — (0:00 – 1:00)
*(Slide: trang tiêu đề)*

Em kính chào thầy/cô ạ. [ngắt]
Hôm nay em xin trình bày đề tài: Ứng dụng AI vào quản trị vận hành. [ngắt]
Em sẽ lấy một ví dụ rất gần gũi, [ngắt] đó là quy trình cấp mã khuyến mãi của một doanh nghiệp bán lẻ, [ngắt] để cho thấy AI giúp quy trình nhanh hơn và nhẹ nhàng hơn như thế nào. [ngắt]
Bài của em dài khoảng hai mươi phút, gồm bảy phần. [ngắt]
Em xin phép bắt đầu ạ.

## Phần 1 — Bối cảnh và mục tiêu — (1:00 – 3:00)
*(Slide: Phần 1 — Bối cảnh & Mục tiêu)*

Trước hết là bối cảnh. [ngắt]
Doanh nghiệp này chạy khuyến mãi qua bốn kênh: [ngắt] Website, ứng dụng di động, landing page, và mã QR tại cửa hàng. [ngắt]
Mỗi khi khách muốn nhận mã, [ngắt] yêu cầu phải đi qua một chuỗi bước: [ngắt] đăng ký, kiểm tra thông tin, kiểm tra điều kiện, phê duyệt, [ngắt] rồi mới cấp và gửi mã cho khách. [ngắt]

Vấn đề nằm ở chỗ: [ngắt] phần lớn các bước này được làm thủ công, [ngắt] bởi bộ phận Marketing và chăm sóc khách hàng. [ngắt]
Hậu quả là khách phải chờ lâu, [ngắt] thời gian xử lý thất thường, [ngắt] và hay lặp lại một cái vòng luẩn quẩn: [ngắt] yêu cầu bổ sung, khách bổ sung, rồi lại kiểm tra lại. [ngắt]

Vì vậy, em đặt ra ba câu hỏi. [ngắt]
Thứ nhất: quy trình thật đang chạy ra sao, và nghẽn ở đâu? [ngắt]
Thứ hai: nên đặt AI vào bước nào? [ngắt]
Thứ ba: làm vậy thì cải thiện được bao nhiêu?

## Phần 2 — Dữ liệu và cách làm — (3:00 – 5:00)
*(Slide: Phần 2 — Dữ liệu & Cách làm)*

Để trả lời một cách khách quan, [ngắt] em dùng một phương pháp tên là Process Mining. [ngắt]
Nói đơn giản, [ngắt] đây là cách dựng lại quy trình thật [ngắt] từ dữ liệu nhật ký mà hệ thống tự ghi lại, [ngắt] chứ không nhìn vào quy trình vẽ trên giấy. [ngắt]

Em có hai bộ dữ liệu, [ngắt] mỗi bộ một nghìn hai trăm hồ sơ. [ngắt]
Một bộ là quy trình cũ, làm tay. [ngắt]
Một bộ là quy trình mới, có AI hỗ trợ. [ngắt]
Cả hai cùng có chín trăm bốn mươi hai hồ sơ thành công, [ngắt] và hai trăm năm mươi tám hồ sơ bị từ chối. [ngắt]
Giữ giống nhau như vậy là để so sánh cho công bằng. [ngắt]

Công cụ em dùng là một thư viện mã nguồn mở của Python, [ngắt] tên là PM4Py, [ngắt] giúp vẽ bản đồ quy trình và đo điểm nghẽn.

## Phần 3 — Quy trình hiện tại và điểm nghẽn — (5:00 – 9:00)
*(Slide: Phần 3.1 — biểu đồ tần suất hoạt động)*

Mời thầy/cô nhìn biểu đồ tần suất này. [ngắt]
Bước "kiểm tra thông tin" chạy tới một nghìn tám trăm sáu mươi mốt lần, [ngắt] trong khi chỉ có một nghìn hai trăm hồ sơ. [ngắt]
Tức là nhiều hồ sơ bị kiểm tra đi kiểm tra lại nhiều lần. [ngắt]
Cụ thể, ba mươi hai phẩy tám phần trăm hồ sơ phải làm lại. [ngắt]

*(Slide: Phần 3.2 — bản đồ quy trình)*

Tiếp theo là bản đồ quy trình do công cụ tự dựng. [ngắt]
Đường nối càng đậm, [ngắt] nghĩa là càng nhiều hồ sơ đi qua lối đó. [ngắt]

*(Slide: Phần 3.3 và 4.2 — điểm nghẽn)*

Và đây là phần quan trọng nhất: điểm nghẽn. [ngắt]
Em đo xem sau mỗi bước, khách phải chờ bao lâu. [ngắt]
Kết quả: chờ lâu nhất là sau bước "yêu cầu bổ sung", [ngắt] gần mười sáu tiếng đồng hồ. [ngắt]
Sau "kiểm tra điều kiện" là gần sáu tiếng. [ngắt]
Sau "phê duyệt" là gần hai tiếng. [ngắt]
Điểm chung của tất cả: [ngắt] đều là những bước con người làm bằng tay.

## Phần 4 — Đo hiệu quả — (9:00 – 13:00)
*(Slide: Phần 4.1 — phân phối thời gian xử lý)*

Giờ ta nhìn vào tổng thời gian xử lý một hồ sơ. [ngắt]
Trung bình là hai mươi tám tiếng rưỡi. [ngắt]
Thậm chí có hồ sơ kéo dài hơn mười hai ngày. [ngắt]
Đây là con số rất đáng lo cho trải nghiệm của khách hàng. [ngắt]

*(Slide: Phần 4.3 — phân bổ công việc)*

Còn về khối lượng công việc: [ngắt]
Riêng bộ phận Marketing đã gánh khoảng một phần ba toàn bộ thao tác, [ngắt] lên tới hơn bốn nghìn lượt. [ngắt]
Rõ ràng bộ phận này đang quá tải, [ngắt] vì phải làm tay quá nhiều.

## Phần 5 — Quy trình mới có AI — (13:00 – 16:00)
*(Slide: Phần 5 — Quy trình mới có AI)*

Từ những điểm nghẽn vừa tìm được, [ngắt] em thiết kế một quy trình mới, [ngắt] đưa AI vào đúng chỗ đang nghẽn. [ngắt]
Bước kiểm tra thông tin: [ngắt] để AI tự đọc và đối chiếu. [ngắt]
Kiểm tra điều kiện: [ngắt] cho hệ thống tự động chạy theo quy tắc. [ngắt]
Phê duyệt: [ngắt] AI đánh giá bất thường rồi tự cấp mã; [ngắt] chỉ hồ sơ nào nghi ngờ mới chuyển cho người. [ngắt]
Yêu cầu bổ sung: [ngắt] gửi tự động cho khách ngay lập tức. [ngắt]

Nguyên tắc của em là: [ngắt] AI lo phần lớn hồ sơ bình thường, [ngắt] còn con người chỉ tập trung vào các trường hợp ngoại lệ. [ngắt]
Nhờ vậy, [ngắt] vừa nhanh, vừa an toàn. [ngắt]
Trong dữ liệu mới, [ngắt] AI tự động hóa được tám mươi tư phẩy bốn phần trăm hồ sơ. [ngắt]
Chỉ còn mười lăm phẩy sáu phần trăm là cần người duyệt.

## Phần 6 — So sánh kết quả — (16:00 – 19:00)
*(Slide: Phần 6 — So sánh AS-IS và TO-BE)*

Và đây là kết quả em tâm đắc nhất. [ngắt]
Thời gian xử lý trung bình giảm từ hai mươi tám phẩy ba lăm giờ, [ngắt] xuống chỉ còn không phẩy chín hai giờ. [ngắt]
Tức là giảm chín mươi bảy phần trăm, [ngắt] nhanh hơn gần ba mươi mốt lần. [ngắt]
Thời gian chờ ở mọi bước giờ đều dưới một tiếng. [ngắt]
Khối lượng việc của Marketing giảm từ hơn bốn nghìn lượt, [ngắt] xuống chỉ còn một trăm tám mươi bảy lượt, [ngắt] tức giảm hơn chín mươi lăm phần trăm. [ngắt]

Nói cách khác: [ngắt] khách hàng nhận mã gần như tức thì, [ngắt] còn nhân viên được giải phóng để làm những việc sáng tạo hơn.

## Phần 7 — Kết luận — (19:00 – 20:00)
*(Slide: Phần 7 — Kết luận & Đề xuất)*

Để kết lại, [ngắt] em rút ra ba bài học. [ngắt]
Một: đo trước, cải tiến sau; [ngắt] phải có dữ liệu thật thì quyết định mới chắc chắn. [ngắt]
Hai: AI hiệu quả nhất khi gắn đúng điểm nghẽn, [ngắt] chứ không phải rải đều khắp nơi. [ngắt]
Ba: vẫn phải giữ con người trong vòng lặp, [ngắt] để kiểm soát rủi ro. [ngắt]

Về lộ trình áp dụng, em đề xuất ba giai đoạn. [ngắt]
Trước hết là AI hóa khâu kiểm tra. [ngắt]
Tiếp theo là tự động cấp và gửi mã. [ngắt]
Cuối cùng là giám sát quy trình liên tục. [ngắt]

Bài trình bày của em đến đây là hết. [ngắt]
Em xin chân thành cảm ơn thầy/cô đã lắng nghe, [ngắt] và em rất mong nhận được góp ý ạ.

---
### Gợi ý thời lượng theo slide
| Phần | Mốc | Thời lượng |
|---|---|---|
| Mở đầu | 0:00 | 1' |
| 1. Bối cảnh | 1:00 | 2' |
| 2. Dữ liệu | 3:00 | 2' |
| 3. Điểm nghẽn | 5:00 | 4' |
| 4. Hiệu quả | 9:00 | 4' |
| 5. AI (TO-BE) | 13:00 | 3' |
| 6. So sánh | 16:00 | 3' |
| 7. Kết luận | 19:00 | 1' |

### Mẹo khi dùng
- **Tự đọc / lồng tiếng:** đọc đúng chỗ `[ngắt]` để nghe tự nhiên; bỏ qua các dòng *(Slide: …)*.
- **Text-to-speech:** trước khi đưa vào công cụ đọc, nên **xóa hết `[ngắt]` và các dòng *(Slide: …)*** (chúng chỉ là chú thích). Nhiều công cụ tự ngắt nghỉ theo dấu câu.
- **Video người ảo (AI avatar):** dán phần lời thoại (đã bỏ chú thích) vào kịch bản của công cụ; chọn 1 nhân vật, giọng tiếng Việt.
