# Kịch bản thuyết trình video (~20 phút)
### Đề tài: Ứng dụng AI vào Quản trị Vận hành — Process Mining

> Tài liệu này là **kịch bản nói** cho video thuyết trình, chia theo từng phần với mốc thời gian. NotebookLM có thể dùng để tạo audio overview hoặc lời dẫn. Giọng văn: nhóm sinh viên trình bày, xưng "chúng em / tụi em".

---

## Mở đầu (0:00 – 1:00)

"Kính chào thầy/cô và các bạn. Nhóm em xin trình bày đề tài **Ứng dụng AI vào quản trị vận hành**. Tụi em sẽ dùng một ví dụ rất gần gũi — quy trình cấp mã khuyến mãi của một doanh nghiệp bán lẻ — để cho thấy AI giúp quy trình nhanh hơn và nhẹ nhàng hơn ra sao. Phần trình bày khoảng 20 phút, gồm 7 phần: bối cảnh, dữ liệu, quy trình hiện tại, đo hiệu quả, quy trình mới có AI, so sánh kết quả, và kết luận."

## Phần 1 — Bối cảnh & Mục tiêu (1:00 – 3:00)

"Doanh nghiệp chạy khuyến mãi qua 4 kênh: Website, Ứng dụng, Landing page và QR tại cửa hàng. Mỗi yêu cầu nhận mã của khách phải đi qua chuỗi bước: đăng ký, kiểm tra thông tin, kiểm tra điều kiện, phê duyệt, rồi cấp và gửi mã.

Vấn đề là phần lớn các bước này **làm thủ công** bởi Marketing và CSKH. Hậu quả: khách chờ lâu, thời gian xử lý thất thường, và hay bị lặp lại cái vòng 'yêu cầu bổ sung — khách bổ sung — kiểm tra lại'.

Tụi em muốn trả lời ba câu hỏi: Thứ nhất, quy trình thật đang chạy ra sao và nghẽn ở đâu? Thứ hai, nên đặt AI vào đâu? Thứ ba, cải thiện được bao nhiêu?"

## Phần 2 — Dữ liệu & Cách làm (3:00 – 5:00)

"Để trả lời khách quan, nhóm em dùng **Process Mining** — tức là dựng lại quy trình thật từ dữ liệu nhật ký mà hệ thống tự ghi, chứ không nhìn quy trình trên giấy.

Nhóm em có hai bộ dữ liệu, mỗi bộ 1.200 hồ sơ: một bộ quy trình cũ làm tay (AS-IS), một bộ quy trình mới có AI (TO-BE). Cả hai cùng có 942 hồ sơ thành công và 258 bị từ chối, để so sánh công bằng.

Công cụ tụi em dùng là thư viện PM4Py của Python để vẽ bản đồ quy trình và đo điểm nghẽn."

## Phần 3 — Quy trình hiện tại & điểm nghẽn (5:00 – 9:00)

"Nhìn biểu đồ tần suất, các bạn sẽ thấy bước *Kiểm tra thông tin* chạy tới 1.861 lần, trong khi chỉ có 1.200 hồ sơ. Nghĩa là nhiều hồ sơ bị kiểm tra đi kiểm tra lại — 32,8% hồ sơ phải làm lại.

Tiếp theo là bản đồ quy trình do công cụ tự dựng — đường càng đậm thì càng nhiều hồ sơ đi qua.

Và đây là phần quan trọng nhất: điểm nghẽn. Tụi em đo xem sau mỗi bước khách phải chờ bao lâu. Kết quả: chờ lâu nhất là sau bước *Yêu cầu bổ sung* — tới gần **16 tiếng**. Rồi *Kiểm tra điều kiện* gần 6 tiếng, *Phê duyệt* gần 2 tiếng. Điểm chung là: tất cả đều là **những bước làm tay của Marketing**."

## Phần 4 — Đo hiệu quả (9:00 – 13:00)

"Về tổng thời gian xử lý một hồ sơ: trung bình **28 tiếng rưỡi**, có hồ sơ kéo dài hơn 12 ngày. Đây là con số rất đáng lo cho trải nghiệm khách hàng.

Về tải công việc: Marketing gánh khoảng một phần ba toàn bộ thao tác, tới hơn 4.000 lượt. Rõ ràng bộ phận này đang quá tải vì phải làm tay quá nhiều."

## Phần 5 — Quy trình mới có AI (13:00 – 16:00)

"Từ các điểm nghẽn vừa tìm được, nhóm em thiết kế quy trình mới, đưa AI vào **đúng chỗ nghẽn**:
- Bước kiểm tra thông tin → AI tự đọc và đối chiếu.
- Kiểm tra điều kiện → tự động theo quy tắc.
- Phê duyệt → AI đánh giá bất thường rồi tự cấp mã, chỉ hồ sơ nghi ngờ mới chuyển cho người.
- Yêu cầu bổ sung → gửi tự động ngay lập tức.

Nguyên tắc là: AI lo phần lớn hồ sơ bình thường, con người chỉ xử lý ngoại lệ. Nhờ vậy vừa nhanh vừa an toàn. Trong dữ liệu mới, AI tự động hóa được **84,4% hồ sơ**, chỉ 15,6% cần người duyệt."

## Phần 6 — So sánh kết quả (16:00 – 19:00)

"Và đây là kết quả tụi em tâm đắc nhất:
- Thời gian xử lý trung bình giảm từ **28,35 giờ xuống 0,92 giờ** — giảm 97%, nhanh hơn gần **31 lần**.
- Thời gian chờ ở mọi bước giờ đều dưới 1 tiếng.
- Khối lượng việc của Marketing giảm từ **4.003 xuống còn 187 lượt** — giảm hơn 95%.

Nghĩa là khách hàng nhận mã gần như tức thì, còn nhân viên được giải phóng để làm việc sáng tạo hơn."

## Phần 7 — Kết luận (19:00 – 20:00)

"Tóm lại, nhóm em rút ra ba bài học: Một là *đo trước, cải tiến sau* — phải có dữ liệu thật. Hai là *AI hiệu quả nhất khi gắn đúng điểm nghẽn*, chứ không rải đều. Ba là *vẫn giữ con người trong vòng lặp* để kiểm soát rủi ro.

Đề xuất lộ trình gồm 3 giai đoạn: trước hết AI hóa khâu kiểm tra, sau đó tự động cấp mã, cuối cùng là giám sát quy trình liên tục.

Nhóm em xin chân thành cảm ơn thầy/cô và các bạn đã lắng nghe. Tụi em rất mong nhận được góp ý ạ."

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
