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
