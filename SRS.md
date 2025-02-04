
##Software Requirements Specification (SRS)
# Khóa Cửa Thông Minh trên Arduino

---

## 1. Giới thiệu
### 1.1 Mục đích  
Tài liệu này mô tả các yêu cầu phần mềm của hệ thống Khóa Cửa Thông Minh sử dụng Arduino, giúp quản lý quyền truy cập bằng mật khẩu, RFID hoặc vân tay, đảm bảo an ninh cho cửa ra vào.  

### 1.2 Phạm vi  
- Hệ thống khóa cửa sử dụng Arduino Uno kết hợp với module RFID và bàn phím số.  
- Cho phép mở khóa bằng mật khẩu, thẻ RFID.  
- Ghi lại lịch sử mở cửa và hiển thị thông tin trên màn hình LCD.  
- Có cảnh báo khi nhập sai nhiều lần hoặc có truy cập trái phép.  
- Hỗ trợ điều khiển từ xa thông qua **ứng dụng di động**.  

### 1.3 Định nghĩa và thuật ngữ  
| Thuật ngữ | Mô tả |
|-----------|-------|
| Arduino Uno | Vi điều khiển chính của hệ thống. |
| RFID | Công nghệ nhận dạng qua sóng vô tuyến. |
| LCD | Màn hình hiển thị thông tin. |
| Buzzer | Còi báo động khi có truy cập trái phép. |

### 1.4 Tài liệu tham khảo  
- [Arduino Official Documentation](https://www.arduino.cc/en/Guide)  
- [IEEE 830-1998 SRS Guide](https://en.wikipedia.org/wiki/Software_requirements_specification)  

---

## 2. Mô tả chung
### 2.1 Góc nhìn hệ thống  
Hệ thống khóa cửa thông minh hoạt động theo mô hình nhúng với Arduino Uno là bộ điều khiển chính. Nó nhận đầu vào từ bàn phím số, RFID, cảm biến vân tay, xử lý dữ liệu và kích hoạt động cơ servo để mở khóa nếu xác thực thành công.  

### 2.2 Chức năng chung  
- Nhập mật khẩu qua bàn phím.  
- Quét thẻ RFID để mở khóa.  
- Nhận diện vân tay.  
- Ghi log truy cập vào bộ nhớ EEPROM.  
- Cảnh báo khi nhập sai quá số lần quy định.  

### 2.3 Ràng buộc  
- Chạy trên Arduino Uno.  
- Điện áp hoạt động: 5V DC.  
- Kết nối tối đa 10.000 người dùng trong bộ nhớ.  
- Giao tiếp UART, SPI, I2C với các module ngoại vi.  

---

## 3. Yêu cầu chức năng  

### 3.1 Xác thực bằng mật khẩu  
#### Mô tả  
- Người dùng nhập mật khẩu trên bàn phím.  
- Nếu đúng, cửa mở; nếu sai quá 3 lần, còi báo động kêu.  

#### Luồng hoạt động
1. Nhập mật khẩu.  
2. Kiểm tra với dữ liệu lưu trữ.  
3. Nếu đúng → Kích hoạt servo mở khóa.  
4. Nếu sai → Thông báo lỗi và đếm số lần nhập sai.  
5. Nếu sai 3 lần → Bật còi báo động.  

### 3.2 Xác thực bằng RFID  
#### Mô tả
- Người dùng quét thẻ RFID.  
- Nếu thẻ hợp lệ, cửa mở.  

#### Luồng hoạt động   
1. Quét thẻ RFID.  
2. Kiểm tra ID thẻ với danh sách lưu trữ.  
3. Nếu hợp lệ → Mở khóa.  
4. Nếu không hợp lệ → Hiển thị cảnh báo.  

### 3.3 Ghi log truy cập  
- Lưu thời gian mở cửa, ID người dùng vào EEPROM.  
- Hiển thị lịch sử truy cập trên LCD hoặc gửi về ứng dụng.  

### 3.4 Cảnh báo xâm nhập  
- Nếu nhập sai mật khẩu quá 3 lần → Bật còi báo động.  
- Nếu thẻ RFID hoặc vân tay không hợp lệ quá 3 lần → Gửi cảnh báo đến ứng dụng.  

---

## 4. Yêu cầu phi chức năng
|   Mã  |                    Mô tả                                       |
|-------|----------------------------------------------------------------|
| NF-01 | Hệ thống phải hoạt động **ổn định 24/7**.                      |
| NF-02 | Độ trễ mở khóa không vượt quá **2 giây**.                      |
| NF-03 | Lưu trữ tối thiểu **10.000 bản ghi**.                          |
| NF-04 | Hỗ trợ giao tiếp **Bluetooth hoặc WiFi** để điều khiển từ xa.  |


---

## 5. Các ràng buộc khác
- Phần cứng: Hệ thống chỉ chạy trên Arduino Uno hoặc các phiên bản tương đương.  
- An toàn: Dữ liệu mật khẩu và vân tay phải được mã hóa.  
- Bảo trì: Có khả năng cập nhật firmware qua OTA (Over-The-Air).  

---

## 6. Phân công nhóm (4 người)  
| Thành viên |               Nhiệm vụ                       |
|------------|----------------------------------------------|
|       A    | Viết chương trình điều khiển Arduino (C++).  |
|       B    | Thiết kế mạch điện và kết nối phần cứng.     |
|       C    | Xây dựng ứng dụng điều khiển từ xa (nếu có). |
|       D    | Viết tài liệu và kiểm thử hệ thống.          |

---

## 7. Tài liệu tham khảo
- Arduino Uno Datasheet  
- [Arduino RFID Module RC522](https://randomnerdtutorials.com/how-to-use-rc522-rfid-module-with-arduino/)  
- [Fingerprint Sensor with Arduino](https://howtomechatronics.com/tutorials/arduino/fingerprint-sensor-with-arduino/)  


