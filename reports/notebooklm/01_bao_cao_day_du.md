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
