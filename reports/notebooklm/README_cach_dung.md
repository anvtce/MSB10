# Hướng dẫn nạp tài liệu vào NotebookLM

Thư mục này chứa các nguồn (sources) để nạp vào **NotebookLM** nhằm tạo video/audio thuyết trình về đề tài *Ứng dụng AI vào Quản trị Vận hành*.

## Các file trong thư mục

| File | Dùng để làm gì | Định dạng |
|---|---|---|
| `00_TAT_CA_trong_1_file.md` / `.html` | **Nạp 1 nguồn duy nhất** — gộp tất cả nội dung | Markdown / HTML |
| `01_bao_cao_day_du.md` / `.html` | Báo cáo đầy đủ (nguồn chính, nhiều số liệu) | Markdown / HTML |
| `02_kich_ban_thuyet_trinh_20phut.md` / `.html` | **Kịch bản độc thoại 1 người** (xưng "em"), câu ngắn có dấu `[ngắt]`, khớp từng slide — dùng để tự đọc / lồng tiếng / tạo video người ảo | Markdown / HTML |
| `03_hoi_dap_QnA.md` / `.html` | Câu hỏi phản biện & câu trả lời | Markdown / HTML |
| `04_so_lieu_va_bieu_do.md` / `.html` | Bảng số liệu + mô tả biểu đồ bằng lời | Markdown / HTML |

> Có cả bản `.md` và `.html`. NotebookLM nhận tốt cả hai; nếu gặp trục trặc với một định dạng, dùng định dạng còn lại. Bạn cũng có thể copy nội dung `.md` dán vào Google Docs rồi nạp Google Docs.

## Cách nạp (các bước)

1. Mở https://notebooklm.google.com và tạo một **Notebook** mới.
2. Bấm **Add source** (Thêm nguồn).
   - Cách gọn nhất: nạp **`00_TAT_CA_trong_1_file.html`** (hoặc `.md`) — một nguồn chứa đủ mọi thứ.
   - Cách chi tiết hơn: nạp **cả 4 file** `01`→`04` thành 4 nguồn riêng, giúp NotebookLM phân biệt rõ "báo cáo", "kịch bản", "Q&A", "số liệu".
3. (Tùy chọn) Nạp thêm các **hình biểu đồ** trong `../figures/` nếu muốn NotebookLM tham chiếu hình. Lưu ý NotebookLM đọc chữ tốt hơn ảnh, nên file `04_so_lieu_va_bieu_do` đã **mô tả mọi biểu đồ bằng lời**.

## Tạo video/audio thuyết trình

- **Audio Overview / Video Overview**: trong NotebookLM bấm **Generate** để tạo bản thuyết minh. Nên đặt gợi ý (customize) theo kiểu: *"Trình bày như một nhóm sinh viên báo cáo đề tài, giọng thân thiện, xưng 'chúng em', dài khoảng 20 phút, tập trung vào hình ảnh/dữ liệu/kết quả, hạn chế thuật ngữ kỹ thuật."*
- **Hỏi đáp**: dùng ô chat để hỏi thử các câu trong `03_hoi_dap_QnA` nhằm luyện phản biện.
- **Dàn ý/Study guide**: NotebookLM có thể tự sinh dàn ý — đối chiếu với `02_kich_ban_thuyet_trinh_20phut` để căn thời lượng.

## Mục tiêu đã đảm bảo trong tài liệu
- Đủ **bối cảnh, phương pháp, dữ liệu, kết quả** để NotebookLM hiểu trọn vẹn đề tài.
- Mọi **con số đều khớp** với notebook phân tích (`../../notebooks/ai_operations_process_mining.ipynb`).
- Có sẵn **kịch bản 20 phút** và **mô tả biểu đồ bằng lời** để bản thuyết minh mạch lạc, đúng trọng tâm.
