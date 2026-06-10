# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** 2A202601005

**Name:** Đặng Hữu Nghĩa

**Date:** 2026-06-10

## 1. Ket qua thi nghiem

Query test:

```text
What is the best electronic product?
```

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Agent: Based on my data, the best choice is Laptop at $1200. | 9 | Clean data đã qua ETL, loại record lỗi, chuẩn hóa category, và giữ lại các sản phẩm hợp lệ. |
| Garbage Data (`garbage_data.csv`) | Agent: Based on my data, the best choice is Nuclear Reactor at $999999. | 2 | Garbage data có outlier quá lớn trong category electronics, làm agent chọn kết quả không phù hợp. |

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Agent simulation trong bài lab hoạt động theo logic rất đơn giản: nếu query có từ "electronic", agent lọc các record có category là electronics, sau đó chọn item có price cao nhất. Với clean data, pipeline đã validate và transform trước khi agent đọc file, nên các record lỗi như price âm, category rỗng, hoặc dữ liệu thiếu đã bị loại bỏ. Kết quả agent trả về Laptop là hợp lý trong tập dữ liệu sạch.

Khi dùng garbage data, chất lượng đầu vào không được kiểm soát. File garbage có duplicate ID, wrong data type như price bằng chữ, null values, price bằng 0, và đặc biệt là outlier Nuclear Reactor có price 999999 trong category electronics. Vì agent chỉ dựa vào price lớn nhất, outlier này làm agent tin rằng Nuclear Reactor là lựa chọn tốt nhất. Đây là lỗi về data quality, không phải chỉ là lỗi prompt. Nếu dữ liệu bị poison, thiếu validation hoặc chưa xử lý outlier, agent có thể đưa ra câu trả lời sai, vô lý, hoặc không an toàn dù câu hỏi của user rất rõ ràng.

## 3. Ket luan

**Quality Data > Quality Prompt?** 
Đồng ý. Prompt tốt giúp agent hiểu yêu cầu, nhưng kết quả cuối cùng vẫn phụ thuộc rất nhiều vào nguồn data. Một pipeline có validation, transform, logging và timestamp sẽ giúp agent dùng dữ liệu đáng tin cậy hơn, dễ debug hơn, và giảm rủi ro trả lời sai khi gặp garbage data.