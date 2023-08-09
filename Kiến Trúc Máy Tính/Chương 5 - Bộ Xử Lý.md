# 5.1 Tổ Chức Của CPU
## 1. Cấu Trúc Cơ Bản Của CPU
- Fetch Instruction: CPU đọc lệnh từ bộ nhớ
- Decode Instruction: Xác định thao tác mà lệnh yêu cầu
- Fetch Data: CPU nhận dữ liệu từ bộ nhớ hoặc các cổng vào ra
- Process Data: Thực hiện các phép toán số hoặc phép toán logic với dữ liệu
- Write Data: Ghi dữ liệu ra bộ nhớ hoặc các cổng vào ra
![[Pasted image 20230809200331.png]]
## 2. Đơn Vị Số Học và Logic (ALU)
Thực hiện phép toán **số học** và **logic**: 
	- Số học: cộng trừ nhân chia đảo dấu
	- Logic: AND, OR, XOR, NOT, Dịch bit
![[Pasted image 20230809200906.png]]
## 3. Đơn Vị Điều Khiển (Control Unit)
- Điều khiển nhận lệnh từ bộ nhớ vào CPU
- Tăng nội dung con trỏ PC để trỏ sang lệnh kế tiếp
- Giải mã lệnh đã được nhận để xác định thao tác mà lệnh yêu cầu
- Phát ra các tín hiệu điều khiển thực hiện lệnh
- Nhận các tín hiệu yêu cầu từ bus hệ thống và đáp ứng các yêu cầu đó
![[Pasted image 20230809201318.png]]
### Các tín hiệu đi đến đơn vị điều khiển
- Clock: tín hiệu nhịp từ mạch tạo dao động bên ngoài
- Lệnh máy từ thanh ghi lệnh đưa đến để giải mã
- Các cờ từ thanh ghi cờ cho biết trạng thái của CPU
- Các tín hiệu yêu cầu từ bus điều khiển
### Các tín hiệu phát ra từ đơn vị điều khiển
Các tín hiệu điều khiển bên trong CPU
- Điều khiển các thanh ghi
- Điều khiển ALU
Các tín hiệu điều khiển bên ngoài CPU
- ĐIều khiển bộ nhớ
- Điều khiển các mô-đun vào-ra
### Các phương pháp thiết kế đơn vị điều khiển
![[Pasted image 20230809202144.png]]

![[Pasted image 20230809202154.png]]

## 4. Hoạt Động Của Chu Trình Lệnh

![[Pasted image 20230809202442.png]]
### Tính địa chỉ của lệnh

# 5.2 Thiết Kế Bộ Xử Lý Theo Kiến Trúc MIPS
# 5.3 Kỹ Thuật Đường Ống Lệnh và Song Song Mức Lệnh
