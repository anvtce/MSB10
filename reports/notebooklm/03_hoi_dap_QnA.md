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
