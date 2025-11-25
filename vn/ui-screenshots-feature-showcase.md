# UI Screenshots & Feature Showcase

# 1. Mục tiêu tài liệu

Tài liệu này cung cấp **các màn hình giao diện chính** của Hydra One nhằm minh họa rõ ràng:

* Trải nghiệm người dùng thực tế
* Quy trình giao dịch và cung cấp thanh khoản
* Các tính năng cốt lõi của hệ thống

Ảnh minh họa sẽ được nhóm phát triển tự chèn vào từng mục theo bố cục dưới đây.


---

# 2. **Màn hình Trang chủ (Home / Dashboard)**

**Mục đích:**

* Cung cấp cái nhìn tổng quan về thị trường và các cặp giao dịch phổ biến.
* Hiển thị thống kê pool, funding rate, volume.

  

 ![](../images/b8fb8c32-b502-448b-9b6e-d46d788db67f.png " =1915x886")


---

 ![](../images/3a6b2768-32fc-44d6-b73d-ddd5b469d704.png " =864x266")

# 3. **Màn hình Kết nối ví (Wallet Connect)**

**Mục đích:** Cho phép người dùng kết nối ví (Nami / Eternl / Lace) Hoặc login bằng Email để thực hiện giao dịch.


 ![](../images/719b6785-c644-4c7b-ac5e-cee4254e3e56.png " =564x467")

 ![](../images/65b37b44-a3bb-4133-af49-09fc2cbe0f3b.png " =556x795")


 ![Deposit from Layer1 to Head hydra(Layer 2)](../images/970d609b-4495-4221-a366-eb13a777a43f.png " =454x357")


 ![Withdraw From Head Hydra(Layer2) to Layer1](../images/24789d4a-c890-46d7-aed5-6db46f45d7df.png " =476x375")

# 4. **Màn hình Giao dịch Perpetual (Trading Screen)**

## 4.1 Biểu đồ giá & chọn cặp giao dịch

**Mô tả:**

* Biểu đồ nến (1m / 5m / 1h / 4h / 1D)
* Orderbook hoặc giá đánh dấu
* Danh sách cặp giao dịch (ADA/USD, BTC/USD, v.v.)

 ![](../images/556907eb-0fcb-4818-95ab-9c03559b6c4b.png " =1505x637")


---

## 4.2 Form mở vị thế (Open Long / Short)

**Mục đích:** Trader nhập thông số để mở vị thế.

**Mô tả chi tiết form:**

* Chọn: Long / Short Market/Limit
* Nhập Collateral
* Chọn Leverage
* Nhập Take Profit
* Nhập Stop Loss
* Tổng chi phí ước tính
* Entry Price dự kiến
* Liquidation Price dự kiến

 ![Long Market](../images/3424054a-9f91-4afb-bfdb-1bf5ca892e54.png " =412x759")


 ![Long Limit](../images/ba6e344c-3049-4ba5-b3c9-25533342c396.png " =407x764")


---


---

# 5. **Màn hình Quản lý vị thế (Positions)**

**Mô tả chi tiết:**

* Danh sách vị thế đang mở
* Entry Price / Mark Price
* Collateral
* Leverage
* PnL (real-time)
* Funding Fee
* TP/SL đang áp dụng
* Nút "Close Position"
* Nút "Edit TP/SL"

 ![](../images/9cd311eb-729f-49fe-a737-67b5fe4033e0.png " =1494x280")


---

# 6. **Màn hình Đóng vị thế (Close Position)**

 ![](../images/fd575c88-c6fb-421d-bbdd-1d22ed4624c0.png " =1501x385")


---

# 7. **Màn hình Lịch sử giao dịch (Order History)**

**Mô tả chi tiết:**

* Danh sách lệnh mở/đóng
* Các lệnh TP/SL tự động
* Lệnh bị thanh lý
* Trạng thái: success / pending / failed
* Timestamp
* Giá trị lệnh

 ![](../images/0c439b9e-8358-4419-bc44-03b1612dcdc4.png " =1497x281")


---

# 8. **Màn hình Cung cấp thanh khoản (Liquidity)**

## 8.1 Giao diện tổng quan LP

**Mô tả:**

* Tổng thanh khoản
* Tỷ suất lợi nhuận dự kiến
* Phân bổ theo cặp

 ![](../images/2fce01ae-b8ef-4163-a227-9d19a5f90abe.png " =1909x761")


---

## 8.2 Form Add Liquidity

**Mô tả:**

* Nhập số lượng ADA / token
* Xem tỷ lệ chia
* Dự đoán LP Token mint ra

 ![](../images/e8b81b3e-5c46-48da-ba22-0f3ec57ea124.png " =457x584")


---

## 8.3 Form Withdraw Liquidity

**Mô tả:**

* Nhập số lượng LP Token muốn rút
* Tính toán số tiền trả về

 ![](../images/eb854531-952d-484a-8f05-e0f964232602.png " =459x578")