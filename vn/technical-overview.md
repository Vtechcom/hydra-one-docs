# TECHNICAL OVERVIEW

# **MÔ TẢ DỰ ÁN HYDRA ONE**


---

## **1. Tên dự án**

**Hydra One – Perpetuals Trading Platform on Hydra Layer 2**


---

## **2.** Giới thiệu Hydra One

Hydra One là một nền tảng giao dịch phái sinh dạng Perpetuals được xây dựng trực tiếp trên Hydra – giải pháp mở rộng Layer 2 của Cardano.\nBằng cách tận dụng khả năng xử lý giao dịch tức thời với chi phí gần bằng 0 của Hydra, Hydra One mang đến trải nghiệm giao dịch vượt trội, minh bạch on-chain nhưng tốc độ ngang Centralized Exchange. Dự án này đặt mục tiêu trở thành minh chứng (real-world demonstration) mạnh mẽ nhất về khả năng ứng dụng của Hydra trong lĩnh vực tài chính phi tập trung.


---

## **3. Vấn đề hiện tại**

Các sàn Perpetuals trên Cardano Layer 1 đang gặp nhiều giới hạn:

* Thời gian xác nhận giao dịch chậm (5 – 20 giây/block).
* Không đáp ứng được nhu cầu giao dịch tần suất cao, độ trễ thấp.
* Phí mạng + phí batcher cao (1.8 – 2.2 ADA/lệnh).
* Trải nghiệm người dùng không thể cạnh tranh với các hệ sinh thái khác như Solana, Arbitrum, hoặc CEX.

**Những người bị ảnh hưởng:**

* Trader thường xuyên mở/đóng vị thế.
* Nhà cung cấp thanh khoản muốn tối ưu doanh thu từ phí giao dịch.

**Tại sao quan trọng:**\nNếu Cardano muốn mở rộng thị trường DeFi, cần có những ứng dụng thể hiện rõ tính vượt trội về tốc độ, chi phí và độ tin cậy. Hydra là giải pháp mạnh mẽ nhưng hiện chưa có sản phẩm thực tế nào chứng minh tiềm năng của nó. Hydra One chính là mảnh ghép chiến lược trong quá trình này.


---

## **4. Giải pháp – Hydra One**

Hydra One cung cấp một sàn giao dịch Perpetuals chạy trực tiếp trên Hydra, mang lại:

* Giao dịch gần tức thì (<1 giây).
* Phí gần như bằng 0 và không cần phí batcher.
* Mô hình minh bạch hoàn toàn on-chain, nhưng tốc độ ngang CEX.
* Trải nghiệm trơn tru, thích hợp cho người dùng chuyên nghiệp.

**Điểm khác biệt:**

* Tăng tốc độ xử lý lên hàng trăm lần so với Layer 1.
* Giảm chi phí giao dịch hơn 95%.
* Dễ dàng mở rộng khi có nhiều Hydra Head.
* Tạo lợi thế cạnh tranh mạnh mẽ cho Cardano trong phân khúc perp trading.

**Hydra One hướng đến**:

* Tái kiến trúc hoàn toàn mô hình Perp DEX của L1 sang L2.
* Xử lý tất cả logic phức tạp (PnL, margin, liquidation, funding) trong Hydra Head.
* Settlement tài sản an toàn trên Cardano L1.
* Kết hợp Smart Contract + Bot + Oracle thành một hệ thống thống nhất, tốc độ cao.


---

## **5. Các tính năng chính**

### **Trader**

* Mở/đóng Long–Short với Market/Limit/Stop-Limit.
* Giao dịch nhiều tài sản trong hệ sinh thái Cardano.
* Dashboard theo dõi PnL, margin, vị thế real-time.
* Độ trễ dưới 1 giây → phù hợp với cả bot trading, scalping.

### **Nhà cung cấp thanh khoản (LP)**

* Thêm/rút thanh khoản linh hoạt.
* Nhận lợi nhuận từ phí giao dịch các khoản thua lỗ từ Trader.
* Công cụ quản lý hiệu suất pool & phân tích rủi ro.

### **Hạ tầng dữ liệu & API**

* Bộ phân tích dữ liệu nâng cao cho trader và LP.
* API mở để đối tác build bot, tích hợp ứng dụng hoặc tạo sản phẩm dịch vụ liên quan.


---

## **6. Giá trị mang lại**

### **Cho người dùng**

* Giảm chi phí mỗi lệnh từ \~2 ADA → gần như bằng 0.
* Giao dịch tốc độ tức thì.
* Trải nghiệm tương đương CEX nhưng không cần tin tưởng trung gian.

### **Cho hệ Cardano**

* Tạo proof-of-concept mạnh mẽ cho Hydra.
* Mở đường cho các dApp tốc độ cao (DEX, gaming, oracle…).
* Thu hút dòng vốn và người dùng mới từ các hệ sinh thái khác.

### **Tác động dài hạn**

* Catalyst có thể chứng minh rằng Hydra không chỉ là khả năng lý thuyết mà thực sự hoạt động với sản phẩm dApp quy mô thực tế.
* Tăng sức hấp dẫn của Cardano đối với nhà đầu tư, dự án DeFi, nhà phát triển.

## 7. Thành phần hệ thống

### **Hydra Execution Layer (L2)**

Đây là trung tâm xử lý toàn bộ hoạt động:

* Khớp lệnh (matching engine)
* Cập nhật PnL
* Tính funding fee
* Kiểm tra margin
* Xử lý liquidation
* Real-time state via snapshots

**Lợi ích**: tốc độ cực nhanh, không phí, deterministic.


---

### **Cardano L1 Wallet**

Đảm nhiệm:

* Nạp tài sản (deposit)
* Rút tài sản (withdraw)


---

### **Off-chain Services**

| Thành phần | Chức năng |
|----|----|
| **API Gateway** | Nhận lệnh từ UI, xác thực dữ liệu |
| **Order Service** | Chuẩn hóa order gửi vào Hydra |
| **Risk Engine** | Tính PnL, TP/SL, liquidation |
| **Matching Engine** | Khớp lệnh trong Hydra |
| **Indexer** | Theo dõi state trong Hydra Head |
| **Oracle Service** | Cập nhật giá thị trường mỗi 1-3s |


---

### **Frontend UI**

* Giao diện giao dịch real-time
* Biểu đồ giá
* Bảng vị thế
* Form mở/đóng lệnh
* Dashboard LP