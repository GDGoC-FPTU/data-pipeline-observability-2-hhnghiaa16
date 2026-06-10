[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=24112725&assignment_repo_type=AssignmentRepo)

# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** danghuunghiahphp@gmail.com

**Student ID:** 2A202601005

**Name:** Đặng Hữu Nghĩa

## Mo ta

Bài lab này xây dựng một ETL pipeline đơn giản cho dữ liệu sản phẩm. Pipeline đọc dữ liệu từ raw_data.json, validate để loại bỏ các record không hợp lệ, transform dữ liệu theo business logic, và load kết quả ra processed_data.csv.

Các rule validation đã implement:

Loại record có price <= 0.
Loại record thiếu hoặc rỗng category.
Ghi log số record được giữ lại và số record bị drop.
Các rule transform đã implement:

Thêm cột discounted_price = price * 0.9.
Chuẩn hóa category sang Title Case.
Thêm cột processed_at để theo dõi thời điểm xử lý.

## Cach chay

### Prerequisites

```bash
pip install pandas pytest
```

Neu dung virtual environment co san trong project tren Windows:

```powershell
.\venv\Scripts\activate
```

### Chay ETL Pipeline

```bash
python solution.py
```

Lenh nay tao hoac cap nhat file `processed_data.csv` trong thu muc goc cua project.

### Chay Agent Simulation

Tao garbage data:

```bash
python generate_garbage.py
```

Chay agent voi clean data va garbage data:

```bash
python agent_simulation.py
```

### Chay test local

```bash
pytest
```

Hoac tren Windows voi venv:

```powershell
.\venv\Scripts\python.exe -m pytest
```

## Cau truc thu muc

```text
solution.py             # ETL pipeline script
raw_data.json           # Input data
processed_data.csv      # Output cua pipeline
generate_garbage.py     # Tao garbage_data.csv cho stress test
agent_simulation.py     # Mo phong agent doc du lieu
experiment_report.md    # Bao cao thi nghiem clean vs garbage data
tests/test_autograder.py # Local/autograder tests
README.md               # Huong dan project
```

## Ket qua

Với raw_data.json, pipeline đọc 5 records. Sau validation, pipeline giữ lại 3 records hợp lệ và drop 2 records lỗi:

Record id 3 bị drop vì price <= 0.
Record id 4 bị drop vì thiếu category.
Output processed_data.csv gồm 3 sản phẩm hợp lệ: Laptop, Chair, Monitor. File output có thêm cột discounted_price và processed_at, đáp ứng yêu cầu observability của lab.