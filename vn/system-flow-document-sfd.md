# System Flow Document (SFD)

# 1. Mục tiêu tài liệu

Tài liệu **SFD – System Flow Document** mô tả:

* Toàn bộ luồng giao dịch giữa **Trader – LP – Bot – Smart Contract – Pool**
* Mô tả chi tiết từng bước xử lý nội bộ
* Dựa trên mô hình 2 giao dịch:
  * **Tx1:** User khởi tạo yêu cầu
  * **Tx2:** Bot xử lý và hoàn thành nghiệp vụ
* Cách các Smart Contract tương tác với nhau:
  * **SC Orders** – lưu yêu cầu & khóa tài sản
  * **SC Manager Position** – quản lý vị thế
  * **SC Pool** – quản lý thanh khoản, funding, PnL

Tài liệu này là nền tảng để đội kỹ thuật xây dựng **Smart Contract**, **Bot Engine**, và **Database**.


---

# 2. Các thành phần hệ thống

| Thành phần | Vai trò |
|----|----|
| **Trader** | Mở/đóng vị thế Long/Short |
| **LP – Liquidity Provider** | Cung cấp & rút thanh khoản |
| **Smart Contract Orders** | Nhận yêu cầu, khóa ADA, lưu datum, mint token |
| **Smart Contract Manager Position** | Lưu trạng thái vị thế đang mở |
| **Smart Contract Pool** | Quản lý tài sản, funding, thanh khoản |
| **Bot Hệ thống** | Tự động tạo Tx2 cho mọi nghiệp vụ |
| **Oracle Price Feed** | Cung cấp giá hiện tại để đóng auto/SL/TP hoặc thanh lý |


---

# 3. **Luồng 1 – Trader mở vị thế (Open Position Flow)**

### **Tx1 – Trader tạo yêu cầu**

* Trader gửi giao dịch mở vị thế.
* ADA collateral bị **khóa trong SC Orders**.
* Orders lưu **datum mô tả lệnh**.
* Orders **mint ra Position Token** để định danh lệnh mở này.

### **Tx2 – Bot xử lý yêu cầu**

* Bot quét Orders theo **FIFO**.
* Bot chuyển:\n✔ Position Token → SC Manager Position\n✔ Collateral → SC Pool\n✔ 2.5 ADA (logic SC yêu cầu) → SC Manager Position
* Manager Position tạo **vị thế mới**.
* Pool cập nhật **tổng thanh khoản**.

### **Sequence Diagram**


```mermaid
sequenceDiagram
    participant Trader
    participant Orders as Smart Contract Orders
    participant Bot as Bot Hệ Thống
    participant Manager as Smart Contract Manager Position
    participant Pool as Smart Contract Pool

    Trader ->> Orders: Tx1. Đặt lệnh mở vị thế
    Note over Orders: Khóa ADA của Trader vào UTxO và lưu datum mô tả yêu cầu
    Orders ->> Orders: Mint token đại diện cho vị thế (Position Token)

    Bot ->> Orders: Tx2. Đọc các yêu cầu mở vị thế
    Orders ->> Manager: Tạo vị thế trên Smart Contract Manager Position
    Orders ->> Pool: Chuyển tiền thế chấp của Trader sang Smart Contract Pool và cập nhật datum cho Pool
```



---

# 4. **Luồng 2 – Trader đóng vị thế (Close Position Flow)**

### **Tx1 – Trader tạo yêu cầu**

* Trader gửi giao dịch đóng.
* Manager chuyển **Position Token + trạng thái vị thế** về Orders.
* Orders lưu yêu cầu trong UTxO.

### **Tx2 – Bot xử lý**

* Bot đọc yêu cầu từ Orders.
* Burn Position Token.
* Tính toán **gốc + lãi/lỗ**.
* Chuyển tiền từ Pool → Trader.
* Pool cập nhật datum mới.

### **Sequence Diagram**

```mermaid
sequenceDiagram
    participant Trader
    participant Orders as Smart Contract Orders
    participant Bot as Bot Hệ Thống
    participant Manager as Smart Contract Manager Position
    participant Pool as Smart Contract Pool

    Trader ->> Manager: Tx1. Đặt lệnh đóng vị thế
    Manager ->> Orders: Chuyển vị thế hiện tại và token sang Smart Contract Orders
    Note over Orders: Lưu yêu cầu đóng vị thế cùng Position Token

    Bot ->> Orders: Tx2. Đọc yêu cầu đóng vị thế từ Smart Contract Orders
    Orders ->> Orders: Burn Position Token
    Pool ->> Trader: Tính toán và chuyển tiền (gốc + lãi/lỗ) về ví Trader
    Pool ->> Pool : Cập nhật datum mới cho Pool
```


---

# 5. **Luồng 3 – Đóng vị thế tự động (Take Profit / Stop Loss)**

### **Tx1 – Bot phát hiện điều kiện TP/SL**

* Bot đọc Manager Position.
* So sánh Current Price với TP/SL.
* Vị thế đạt điều kiện được chuyển về Orders.

### **Tx2 – Bot xử lý**

* Burn Position Token.
* Chuyển tiền từ Pool → Trader.
* Cập nhật datum Pool.

### **Sequence Diagram**

```mermaid
sequenceDiagram
    participant Trader
    participant Bot as Bot Hệ Thống
    participant Manager as Smart Contract Manager Position
    participant Orders as Smart Contract Orders
    participant Pool as Smart Contract Pool

    Bot ->> Manager: Tx1. Quét toàn bộ vị thế có thiết lập Take Profit / Stop Loss
    Note over Manager: So sánh giá hiện tại (Current Price)<br/>với giá Take Profit và Stop Loss
    Manager ->> Orders: Chuyển các vị thế cần đóng về Smart Contract Orders
    Note over Orders: Lưu yêu cầu đóng vị thế tự động kèm Position Token

    Bot ->> Orders: Tx2. Đọc các yêu cầu đóng vị thế tự động từ Smart Contract Orders
    Orders ->> Orders: Burn Position Token
    Pool->> Trader: Tính toán và chuyển tiền (gốc + lãi/lỗ) về ví Trader
    Pool ->> Pool : Cập nhật datum mới của Pool
```



---

# 6. **Luồng 4 – Thanh lý tài sản (Liquidation Flow)**

### **Tx1 – Bot phát hiện vị thế bị thanh lý**

* So sánh Current Price với Liquidation Price.
* Chuyển vị thế + token về Orders.

### **Tx2 – Bot thực hiện thanh lý**

* Burn token.
* Cập nhật Pool (collateral mất hoặc giảm).
* Kết thúc vị thế.

### **Sequence Diagram**

```mermaid
sequenceDiagram
    participant Bot as Bot Hệ Thống
    participant Manager as Smart Contract Manager Position
    participant Orders as Smart Contract Orders
    participant Pool as Smart Contract Pool Thanh Khoản

    Bot ->> Manager: Tx1. Quét toàn bộ vị thế đạt ngưỡng thanh lý
    Note over Manager: So sánh giá hiện tại (Current Price)<br/>với giá thanh lý (Liquidation Price)
    Manager ->> Orders: Chuyển các vị thế cần thanh lý về Smart Contract Orders
    Note over Orders: Lưu yêu cầu thanh lý kèm Position Token

    Bot ->> Orders: Tx2. Đọc yêu cầu thanh lý từ Smart Contract Orders
    Orders->> Orders: Burn Position Token
    Bot ->> Pool: Cập nhật datum mới của Pool (điều chỉnh số dư thanh khoản)
```



---

# 7. **Luồng 5 – Provider thêm thanh khoản (Add Liquidity)**

### **Tx1 – LP gửi yêu cầu**

* LP gửi ADA vào Orders.
* Orders lưu datum mô tả yêu cầu.

### **Tx2 – Bot xử lý**

* Bot đọc Orders.
* Chuyển ADA → Pool.
* Pool mint LP Token trả về Provider.

### **Sequence Diagram**

```mermaid
sequenceDiagram
    participant Provider
    participant Orders as Smart Contract Orders
    participant Bot as Bot Hệ Thống
    participant Pool as Smart Contract Pool Thanh Khoản

    Provider ->> Orders: Tx1. Gửi yêu cầu Add Liquidity
    Note over Orders: Khóa ADA của Provider vào UTxO<br/>và lưu datum mô tả yêu cầu Add Liquidity

    Bot ->> Orders: Tx2. Quét và đọc toàn bộ yêu cầu Add Liquidity
    Orders->> Pool: Chuyển tài sản vào Pool Thanh Khoản
    Pool ->> Pool : Cập nhật datum mới phản ánh thanh khoản hiện tại
    Pool ->> Provider: Mint token thanh khoản (LP Token) trả về ví Provider
```


---

# 8. **Luồng 6 – Provider rút thanh khoản (Remove Liquidity)**

### **Tx1 – Provider gửi yêu cầu**

* LP Token bị khóa vào Orders.
* Orders lưu thông tin rút.

### **Tx2 – Bot xử lý**

* Burn LP Token.
* Chuyển ADA tương ứng về Provider.
* Pool cập nhật datum.

### **Sequence Diagram**

```mermaid
sequenceDiagram
    participant Provider
    participant Orders as Smart Contract Orders
    participant Bot as Bot Hệ Thống
    participant Pool as Smart Contract Pool Thanh Khoản

    Provider ->> Orders: Tx1. Gửi yêu cầu Whidraw Liquidity
    Note over Orders: Khóa LP Token của Provider vào UTxO<br/>và lưu datum mô tả yêu cầu Remove Liquidity

    Bot ->> Orders: Tx2. Quét và đọc toàn bộ yêu cầu Remove Liquidity
    Orders->> Orders: Burn LP Token và rút phần tài sản tương ứng khỏi Pool
    Pool ->> Provider: Chuyển ADA / tài sản tương ứng về ví Provider
    Pool ->> Pool: Cập nhật datum mới phản ánh thanh khoản giảm
```


---

# 9. **Tổng kết về kiến trúc xử lý**

| Loại nghiệp vụ | Tx1 bởi User | Tx2 bởi Bot | Token liên quan | Pool thay đổi? |
|----|----|----|----|----|
| Mở vị thế | Trader | Bot | Position Token (mint) | Có |
| Đóng vị thế | Trader | Bot | Position Token (burn) | Có |
| Auto TP/SL | Bot | Bot | Position Token (burn) | Có |
| Thanh lý | Bot | Bot | Position Token (burn) | Có |
| Add Liquidity | Provider | Bot | LP Token (mint) | Có |
| Remove Liquidity | Provider | Bot | LP Token (burn) | Có |