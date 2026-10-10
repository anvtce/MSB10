# Ứng dụng AI vào Quản trị Vận hành — Báo cáo đầy đủ
### Phân tích quy trình bằng Process Mining (Ví dụ: Quy trình cấp mã khuyến mãi)

> Tài liệu này là nguồn chính cho NotebookLM. Nội dung được viết dạng văn xuôi, đầy đủ số liệu thực tế, để NotebookLM có thể tạo bản tóm tắt, audio overview và kịch bản thuyết trình chính xác.

---

## 1. Bối cảnh và vấn đề

Một doanh nghiệp bán lẻ vận hành nhiều chương trình khuyến mãi, cấp mã giảm giá cho khách hàng thông qua bốn kênh: Website, Ứng dụng di động, Landing page và QR tại cửa hàng. Mỗi yêu cầu nhận mã của khách đi qua một chuỗi bước nghiệp vụ: khách đăng ký, hệ thống ghi nhận, bộ phận kiểm tra thông tin, kiểm tra điều kiện, phê duyệt, rồi cấp và gửi mã cho khách.

Vấn đề là phần lớn các bước này được xử lý **thủ công** bởi bộ phận Marketing và Chăm sóc khách hàng (CSKH). Hệ quả: thời gian xử lý kéo dài và thất thường, khách phải chờ lâu, và quy trình thường xuyên rơi vào vòng lặp "yêu cầu khách bổ sung thông tin — khách bổ sung — kiểm tra lại" (hiện tượng *làm lại*, tiếng Anh là *rework*).

Nhóm nghiên cứu đặt ra ba câu hỏi:
1. Quy trình thực tế đang diễn ra như thế nào, và điểm nghẽn nằm ở đâu?
2. Nên đưa AI vào bước nào để tạo giá trị lớn nhất?
3. Quy trình mới có AI cải thiện được bao nhiêu về thời gian và nhân lực?

## 2. Phương pháp: Process Mining

Thay vì mô tả quy trình "trên giấy", nhóm sử dụng **Process Mining** — kỹ thuật dựng lại quy trình **thực tế** từ dữ liệu nhật ký sự kiện (event log) mà hệ thống tự ghi lại. Mỗi dòng nhật ký là một bước đã diễn ra, gồm: mã hồ sơ (Case ID), tên hoạt động (Activity), thời điểm (Timestamp), người/bộ phận thực hiện (Resource), kênh, chương trình, và kết quả cuối (Thành công/Từ chối).

Công cụ sử dụng:
- **PM4Py** (thư viện Python mã nguồn mở): dựng bản đồ quy trình (Directly-Follows Graph), phân tích hiệu năng, phát hiện các biến thể quy trình.
- **pandas**: tính toán các chỉ số.
- **matplotlib / seaborn**: trực quan hóa.

Ưu điểm của Process Mining: nó dựa trên dấu vết số có sẵn, nên chỉ ra **chính xác và khách quan** nơi quy trình bị chậm — làm cơ sở để quyết định "AI nên đặt ở đâu".

## 3. Dữ liệu

Nhóm phân tích hai bộ nhật ký, mỗi bộ gồm **1.200 hồ sơ** khách hàng:
- **AS-IS** (quy trình hiện tại, thủ công): 12.267 sự kiện.
- **TO-BE** (quy trình mới, có AI): 12.847 sự kiện.

Cả hai kịch bản giữ cùng kết quả nghiệp vụ để so sánh công bằng: **942 hồ sơ thành công** và **258 hồ sơ bị từ chối**.

Phân bố theo kênh (trên dữ liệu AS-IS): Website 5.399, Ứng dụng 3.471, Landing page 2.346, QR tại cửa hàng 1.051 sự kiện. Phân bố theo chương trình khá đều nhau: MEMBER15, FREESHIP, WELCOME10, SUMMER20 (mỗi chương trình khoảng 3.000 sự kiện).

## 4. Quy trình hiện tại (AS-IS) và điểm nghẽn

### 4.1. Dấu hiệu làm lại
Khi đếm số lần mỗi hoạt động xuất hiện, bước *Kiểm tra thông tin* chạy tới **1.861 lần** trên 1.200 hồ sơ — cao hơn hẳn số hồ sơ. Đây là bằng chứng của việc **làm lại**: nhiều hồ sơ phải kiểm tra lại sau khi khách bổ sung thông tin. Cụ thể, **32,8% hồ sơ** phải kiểm tra lại ít nhất một lần.

### 4.2. Thời gian xử lý
Tổng thời gian xử lý một hồ sơ (cycle time) ở quy trình cũ:
- Trung bình: **28,35 giờ**
- Trung vị: 12,61 giờ
- P90 (90% hồ sơ xong trong khoảng này): **60,44 giờ**
- Lâu nhất: 297,73 giờ (hơn 12 ngày)

### 4.3. Điểm nghẽn — chờ lâu nhất sau bước nào
Đo khoảng thời gian khách phải chờ sau mỗi bước, các điểm nghẽn lớn nhất ở quy trình cũ là:
- Sau *Yêu cầu bổ sung*: chờ trung bình **15,86 giờ**
- Sau *Kiểm tra điều kiện*: **5,77 giờ**
- Sau *Ghi nhận đăng ký*: 4,81 giờ
- Sau *Bổ sung thông tin*: 4,31 giờ
- Sau *Kiểm tra thông tin*: 3,00 giờ
- Sau *Phê duyệt*: 1,92 giờ

Điểm chung: tất cả điểm nghẽn đều rơi vào **các bước làm tay của Marketing và CSKH**.

### 4.4. Ai đang gánh việc
Ở quy trình cũ, khối lượng thao tác phân bổ: Hệ thống 4.542, **Marketing 4.003**, Khách hàng 2.803, CSKH 919 sự kiện. Marketing gánh khoảng một phần ba toàn bộ công việc.

## 5. Quy trình mới có AI (TO-BE)

Dựa trên phân tích trên, nhóm đưa AI vào **đúng các bước đang gây nghẽn**:

| Bước cũ (làm tay) | Đang nghẽn | Thay bằng (TO-BE) |
|---|---|---|
| Kiểm tra thông tin (Marketing) | làm lại 32,8%, chờ ~3h | **AI kiểm tra thông tin** tự động đọc và đối chiếu |
| Kiểm tra điều kiện (Marketing) | chờ ~5,8h | **Tự động kiểm tra điều kiện** theo quy tắc |
| Phê duyệt (Marketing) | chờ ~1,9h | **AI đánh giá bất thường** + tự động cấp mã; chỉ hồ sơ nghi ngờ mới chuyển người |
| Yêu cầu bổ sung (làm tay) | chờ ~15,9h | **Yêu cầu bổ sung tự động** ngay lập tức |
| Xử lý ngoại lệ | — | **Marketing xét duyệt ngoại lệ** (giữ con người trong vòng lặp) |

Nguyên tắc thiết kế: **AI xử lý phần lớn hồ sơ thông thường, con người tập trung vào ngoại lệ** (mô hình human-in-the-loop). Nhờ vậy vừa nhanh vừa kiểm soát rủi ro. Trên dữ liệu TO-BE, **1.013 hồ sơ (84,4%)** được AI xử lý tự động hoàn toàn, chỉ **187 hồ sơ (15,6%)** cần người xét duyệt.

## 6. Kết quả so sánh trước và sau

### 6.1. Thời gian xử lý giảm mạnh
Cycle time trung bình giảm từ **28,35 giờ xuống 0,92 giờ** — tức giảm khoảng **97%**, nhanh hơn gần **31 lần**. P90 giảm từ 60,44 giờ xuống 2,02 giờ.

Ở quy trình mới, thời gian chờ sau mỗi bước đều dưới 1 giờ: lâu nhất là sau *Yêu cầu bổ sung tự động* cũng chỉ 0,92 giờ (so với 15,86 giờ trước đây).

### 6.2. Giải phóng nhân lực
Khối lượng thao tác của Marketing giảm từ **4.003 xuống còn 187 lượt** — giảm hơn 95%. Phần việc chuyển sang Hệ thống (7.393) và AI / Hệ thống (2.674). Nhân lực Marketing được giải phóng để làm công việc giá trị cao hơn (sáng tạo chiến dịch, phân tích khách hàng).

### 6.3. Giảm làm lại
Tổng số lần kiểm tra thông tin giảm từ 1.861 xuống 1.651 nhờ AI kiểm tra chính xác hơn ngay từ đầu.

## 7. Kết luận và đề xuất

**Kết quả chính:**
- Thời gian xử lý trung bình giảm ~97% (28,35h → 0,92h), nhanh hơn ~31 lần.
- Điểm nghẽn được xác định khách quan bằng Process Mining, nằm ở các bước làm tay.
- Đặt AI đúng điểm nghẽn giúp tự động hóa 84,4% hồ sơ.
- Giảm tải Marketing từ 4.003 xuống 187 lượt thao tác.

**Ba bài học về quản trị vận hành:**
1. Đo trước, cải tiến sau — quyết định dựa trên dữ liệu thật, không phỏng đoán.
2. AI hiệu quả nhất khi gắn đúng điểm nghẽn, không rải đều.
3. Giữ con người trong vòng lặp để cân bằng tốc độ và kiểm soát rủi ro.

**Lộ trình áp dụng đề xuất:**
- Giai đoạn 1: Đưa AI vào kiểm tra thông tin và điều kiện (chỗ nghẽn lớn nhất).
- Giai đoạn 2: Tự động cấp/gửi mã và cảnh báo bất thường.
- Giai đoạn 3: Theo dõi liên tục bằng Process Mining để phát hiện quy trình bị lệch.

---
*Phân tích thực hiện bằng Python + PM4Py trên dữ liệu thực tế: 1.200 hồ sơ cho mỗi kịch bản.*


# ════════════════════

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


# ════════════════════

# Hỏi & Đáp (Q&A) — chuẩn bị phản biện
### Đề tài: Ứng dụng AI vào Quản trị Vận hành — Process Mining

> Tài liệu giúp NotebookLM và nhóm chuẩn bị trả lời câu hỏi khi trình bày/phản biện. Mỗi câu đều dựa trên số liệu thực tế trong báo cáo.

---

**H1: Process Mining là gì, khác gì với vẽ sơ đồ quy trình thông thường?**
Process Mining dựng lại quy trình **thực tế** từ dữ liệu nhật ký hệ thống đã ghi, nên phản ánh đúng những gì diễn ra — kể cả các vòng lặp, ngoại lệ. Sơ đồ quy trình thông thường là quy trình "lý tưởng trên giấy", thường bỏ sót chính những chỗ gây nghẽn.

**H2: Vì sao khẳng định điểm nghẽn nằm ở các bước thủ công?**
Nhóm đo thời gian chờ trung bình sau mỗi bước. Ba bước chờ lâu nhất đều do Marketing/CSKH làm tay: *Yêu cầu bổ sung* 15,86 giờ, *Kiểm tra điều kiện* 5,77 giờ, *Phê duyệt* 1,92 giờ. Đây là bằng chứng định lượng, không phải cảm tính.

**H3: Dữ liệu TO-BE là thật hay mô phỏng?**
Đây là dữ liệu kịch bản dùng để minh họa tác động khi áp dụng AI, được giữ cùng số hồ sơ (1.200) và cùng tỉ lệ kết quả (942 thành công / 258 từ chối) như AS-IS để so sánh công bằng. Mục tiêu là thể hiện phương pháp và độ lớn cải thiện khi tự động hóa đúng điểm nghẽn.

**H4: Giảm 97% thời gian có bị thổi phồng không?**
Con số đến từ việc so sánh cycle time trung bình: 28,35 giờ (cũ) so với 0,92 giờ (mới). Phần lớn thời gian cũ là **thời gian chờ** giữa các bước thủ công (ví dụ chờ 16 giờ sau yêu cầu bổ sung), chứ không phải thời gian xử lý thực. Khi tự động hóa, các khoảng chờ này gần như biến mất — nên mức giảm lớn là hợp lý.

**H5: AI có thay thế hoàn toàn con người không?**
Không. Mô hình là **human-in-the-loop**: AI xử lý 84,4% hồ sơ thông thường, còn 15,6% hồ sơ nghi ngờ/ngoại lệ vẫn chuyển cho Marketing xét duyệt. Con người tập trung vào việc khó và giá trị cao.

**H6: Rủi ro khi để AI tự duyệt là gì, kiểm soát ra sao?**
Rủi ro là AI duyệt sai (bỏ sót gian lận hoặc từ chối nhầm khách thật). Kiểm soát bằng: (1) bước *AI đánh giá bất thường* để đẩy hồ sơ nghi ngờ sang người; (2) giữ con người duyệt ngoại lệ; (3) giám sát liên tục bằng Process Mining (conformance checking) để phát hiện quy trình đi lệch.

**H7: Chi phí triển khai AI có xứng đáng không?**
Lợi ích đo được: giảm 95% tải thao tác của Marketing (4.003 → 187 lượt) và rút ngắn thời gian phục vụ khách ~31 lần. Lộ trình chia 3 giai đoạn giúp triển khai dần, giảm rủi ro và chi phí ban đầu.

**H8: Vì sao bước Kiểm tra thông tin xuất hiện 1.861 lần?**
Vì nhiều hồ sơ thiếu thông tin, phải yêu cầu khách bổ sung rồi **kiểm tra lại** — mỗi lần lặp là một lần đếm thêm. 32,8% hồ sơ rơi vào vòng lặp này. AI đọc và đối chiếu chính xác hơn ngay từ đầu nên giảm được số lần kiểm tra (xuống 1.651).

**H9: Phương pháp này áp dụng được cho quy trình khác không?**
Có. Process Mining áp dụng cho bất kỳ quy trình nào có ghi nhật ký: xử lý đơn hàng, duyệt hồ sơ vay, chăm sóc khách hàng, logistics... Cách làm giống nhau: dựng quy trình thật → tìm điểm nghẽn → đặt AI/tự động hóa đúng chỗ → đo lại.

**H10: Công cụ PM4Py có miễn phí không, cần hạ tầng gì?**
PM4Py là thư viện Python **mã nguồn mở, miễn phí**. Chỉ cần Python và máy tính thông thường; không cần hạ tầng đặc biệt cho phân tích ở quy mô này.


# ════════════════════

# Số liệu & Mô tả biểu đồ bằng lời
### Đề tài: Ứng dụng AI vào Quản trị Vận hành — Process Mining

> Tài liệu cung cấp số liệu dạng bảng và **mô tả từng biểu đồ bằng lời**, giúp NotebookLM hiểu nội dung các hình ảnh mà không cần nhìn trực tiếp. Dùng kèm các file hình trong thư mục `reports/figures/`.

---

## A. Bảng số liệu tổng hợp

### A1. Quy mô dữ liệu
| | AS-IS (cũ) | TO-BE (mới) |
|---|---|---|
| Số hồ sơ | 1.200 | 1.200 |
| Số sự kiện | 12.267 | 12.847 |
| Hồ sơ thành công | 942 | 942 |
| Hồ sơ từ chối | 258 | 258 |

### A2. Thời gian xử lý một hồ sơ (cycle time, đơn vị: giờ)
| Chỉ số | AS-IS | TO-BE | Mức giảm |
|---|---|---|---|
| Trung bình | 28,35 | 0,92 | ~97% |
| Trung vị | 12,61 | 0,50 | ~96% |
| P90 | 60,44 | 2,02 | ~97% |
| Lớn nhất | 297,73 | 6,02 | ~98% |

Nhanh hơn trung bình khoảng **31 lần**.

### A3. Điểm nghẽn — thời gian chờ trung bình sau mỗi bước (giờ)
| Bước (AS-IS) | Thời gian chờ | | Bước (TO-BE) | Thời gian chờ |
|---|---|---|---|---|
| Yêu cầu bổ sung | 15,86 | | Yêu cầu bổ sung tự động | 0,92 |
| Kiểm tra điều kiện | 5,77 | | AI đánh giá bất thường | 0,19 |
| Ghi nhận đăng ký | 4,81 | | Tự động gửi mã | 0,10 |
| Bổ sung thông tin | 4,31 | | Bắt đầu | 0,08 |
| Kiểm tra thông tin | 3,00 | | Tự động kiểm tra điều kiện | 0,05 |
| Phê duyệt | 1,92 | | Ghi nhận đăng ký | 0,05 |

### A4. Phân bổ công việc theo nguồn lực (số sự kiện)
| Nguồn lực | AS-IS | TO-BE |
|---|---|---|
| Hệ thống | 4.542 | 7.393 |
| Marketing | 4.003 | **187** |
| Khách hàng | 2.803 | 2.593 |
| CSKH | 919 | 0 |
| AI / Hệ thống | 0 | 2.674 |

### A5. Mức độ tự động hóa (TO-BE)
- Hồ sơ AI xử lý tự động hoàn toàn: **1.013 (84,4%)**
- Hồ sơ cần người xét duyệt (ngoại lệ): **187 (15,6%)**

### A6. Làm lại (rework)
| | AS-IS | TO-BE |
|---|---|---|
| Tỷ lệ hồ sơ phải kiểm tra lại | 32,8% | 32,8% |
| Tổng số lần kiểm tra thông tin | 1.861 | 1.651 |

### A7. Phân bố theo kênh và chương trình (AS-IS)
- Kênh: Website 5.399 · Ứng dụng 3.471 · Landing page 2.346 · QR tại cửa hàng 1.051
- Chương trình: MEMBER15 3.148 · FREESHIP 3.054 · WELCOME10 3.034 · SUMMER20 3.031

---

## B. Mô tả các biểu đồ (bằng lời)

**Hình 1 — Tần suất hoạt động (2 biểu đồ cột ngang, AS-IS và TO-BE).**
Biểu đồ cột ngang so sánh số lần xuất hiện của mỗi bước. Ở AS-IS, cột *Kiểm tra thông tin* dài nhất (1.861), vượt qua đường kẻ đứt đánh dấu mốc 1.200 hồ sơ — cho thấy làm lại. Ở TO-BE, các cột cân đối hơn và xuất hiện các bước mới có chữ "AI" và "Tự động".

**Hình 2 & 3 — Bản đồ quy trình (Directly-Follows Graph) cho AS-IS và TO-BE.**
Dạng sơ đồ hộp-mũi tên: mỗi hộp là một bước, mũi tên là luồng chuyển từ bước này sang bước kia, con số trên mũi tên là số hồ sơ đi qua. AS-IS có nhiều nhánh phụ và vòng lặp quay lại bước kiểm tra. TO-BE gọn hơn, luồng chính chạy thẳng qua các bước tự động, chỉ tách nhánh nhỏ cho ngoại lệ.

**Hình 4 — Phân phối thời gian xử lý (histogram chồng).**
Trục ngang là số giờ, trục dọc là số hồ sơ. Phân phối AS-IS (màu đỏ cam) trải rộng về phía phải, nhiều hồ sơ mất hàng chục giờ. Phân phối TO-BE (màu xanh) dồn sát về 0 — gần như mọi hồ sơ xong trong vòng 1-2 giờ.

**Hình 5 — Điểm nghẽn AS-IS (cột ngang, kèm nhãn giờ).**
Xếp hạng các bước theo thời gian chờ sau đó. Cột dài nhất là *Yêu cầu bổ sung* với 15,9 giờ, rồi giảm dần. Trực quan cho thấy nghẽn tập trung ở các bước làm tay.

**Hình 6 — Phân bổ công việc theo nguồn lực (2 biểu đồ tròn).**
Hai hình tròn so sánh tỉ lệ công việc. AS-IS: Marketing chiếm miếng lớn (~1/3). TO-BE: miếng Marketing teo lại còn rất nhỏ, phần lớn chuyển sang Hệ thống và AI.

**Hình 7 — So sánh cycle time (cột đôi).**
Hai cặp cột so sánh thời gian trung bình và P90. Cột AS-IS (đỏ cam) cao vượt trội; cột TO-BE (xanh) thấp sát đáy. Nhãn trên cột: 28,3h vs 0,9h (trung bình), 60,4h vs 2,0h (P90).
