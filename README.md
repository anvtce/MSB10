# AI in Operations Management — Process Mining Report

Phân tích quy trình cấp mã khuyến mãi bằng **Process Mining**, so sánh quy trình thủ công
(AS-IS) với quy trình tích hợp AI (TO-BE). Báo cáo trình bày trong ~20 phút, có biểu đồ,
dữ liệu và kết quả định lượng.

## Cấu trúc thư mục

```
.
├── data/
│   ├── raw/          # Dữ liệu gốc (bất biến)
│   │   ├── as_is.csv     # Event log quy trình hiện tại
│   │   ├── as_is.bpmn
│   │   ├── to_be.csv     # Event log quy trình có AI
│   │   └── to_be.bpmn
│   └── processed/    # Dữ liệu trung gian (sinh ra khi chạy)
├── notebooks/
│   └── ai_operations_process_mining.ipynb   # Notebook báo cáo chính
├── reports/
│   └── figures/      # Biểu đồ & bản đồ quy trình xuất ra
├── pyproject.toml    # Khai báo dependencies (quản lý bằng uv)
├── uv.lock
└── README.md
```

## Công cụ

- **PM4Py** — process discovery (Directly-Follows Graph), phân tích variants & hiệu năng
- **pandas** — xử lý event log, tính KPI
- **matplotlib / seaborn** — trực quan hóa
- **Graphviz** — render bản đồ quy trình (`brew install graphviz`)
- Quản lý môi trường: **uv** (Python 3.12)

## Chạy báo cáo

```bash
# Cài dependencies
uv sync

# Mở notebook
uv run jupyter lab notebooks/ai_operations_process_mining.ipynb

# Hoặc chạy toàn bộ không cần mở
uv run jupyter nbconvert --to notebook --execute --inplace \
    notebooks/ai_operations_process_mining.ipynb
```

> Notebook tự xác định project root nên có thể chạy từ bất kỳ thư mục nào.
> Đường dẫn dữ liệu (`data/raw/`) và hình (`reports/figures/`) được resolve tự động.

## Kết quả chính

- Cycle time trung bình: **28.3h → 0.9h** (giảm ~97%, nhanh hơn ~31 lần)
- Điểm nghẽn AS-IS: các bước thủ công của Marketing (yêu cầu bổ sung, kiểm tra điều kiện, phê duyệt)
- Khối lượng việc Marketing: **4003 → 187** sự kiện (phần còn lại giao AI / Hệ thống)
