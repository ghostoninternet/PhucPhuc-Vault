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
### Nhận Lệnh
>1. CPU đưa địa chỉ của lệnh cần nhận từ bộ đếm chương trình PC ra bus địa chỉ
>2. CPU phát tín hiệu điều khiển đọc bộ nhớ
>3. Lệnh từ bộ nhớ được đặt lên bus dữ liệu và được CPU copy vào thanh ghi lệnh IR
>4. CPU tăng nội dung PC để trỏ sang lệnh kế tiếp
![[Pasted image 20230809210020.png]]
### Giải Mã Lệnh
>1. Lệnh từ thanh ghi lệnh IR được đưa đến đơn vị điều khiển
>2. Lệnh từ thanh ghi lệnh IR được đưa đến đơn vị điều khiển
>3. Giải mã lệnh xảy ra bên trong CPU
### Nhận Dữ Liệu Từ Bộ Nhớ
>1. CPU đưa địa chỉ của toán hạng ra bus địa chỉ
>2. CPU phát tín hiệu điều khiển đọc
>3. Toán hạng được đọc vào CPU
>4. Tương tự như nhận lệnh
![[Pasted image 20230809210352.png]]
### Thực Hiện Lệnh
>Có nhiều dạng tùy thuộc vào lệnh
>Có thể là:
>- Đọc / Ghi ra bộ nhớ
>- Vào / Ra
>- Chuyển giữa các thanh ghi
>- Phép toán số học / logic
>- Chuyển điều khiển (rẽ nhánh)
### Ghi toán hạng
> 1. CPU đưa địa chỉ ra bus địa chỉ
> 2. CPU đưa dữ liệu cần ghi ra bus dữ liệu
> 3. CPU phát tín hiệu điều khiển ghi
> 4. Dữ liệu trên bus dữ liệu được copy đến vị trí xác định
![[Pasted image 20230809210740.png]]
### Ngắt
>1. Nội dung của bộ đếm chương trình PC (địa chỉ trở về sau khi ngắt) được đưa ra bus dữ liệu
>2. CPU đưa địa chỉ (thường được lấy từ con trỏ ngăn xếp SP) ra bus địa chỉ
>3. CPU phát tín hiệu điều khiển ghi bộ nhớ
>4. Địa chỉ trở về trên bus dữ liệu được ghi ra vị trí xác định (ở ngăn xếp)
>5. Địa chỉ lệnh đầu tiên của chương trình con điều khiển ngắt được nạp vào PC
![[Pasted image 20230809210923.png]]
# 5.2 Thiết Kế Bộ Xử Lý Theo Kiến Trúc MIPS
### Tổng Quan Hóa Quá Trình Thực Hiện Các Lệnh
- Hai bước đầu tiên với mỗi lệnh:
	- Đưa địa chỉ từ bộ đếm chương trình PC đến bộ nhớ lệnh, tìm và nhận lệnh từ bộ nhớ này
	- Sử dụng các số hiệu thanh ghi trong lệnh để chọn và đọc một hoặc hai thanh ghi:
		- Lệnh lw: đọc 1 thanh ghi
		- Các lệnh khác (không kể lệnh jump): đọc 2 thanh ghi

- Các bước tiếp theo tùy thuộc vào loại lệnh:
	- Sử dụng ALU hoặc bộ cộng Add để: 
		- Thực hiện lệnh số học / logic
		- So sánh trong branch
		- Tính địa chỉ đích
		- Tính địa chỉ ngăn nhớ
	- Truy cập bộ nhớ dữ liệu với lệnh load/store
	- Ghi dữ liệu đến thanh ghi đích

- Thay đổi nội dung bộ đếm chương trình PC:
	- Với các lệnh rẽ nhánh (branch), tùy thuộc vào kết quả so sánh
		- PC <- địa chỉ đích
		- PC <- PC + 4
	- Với các lệnh còn lại (không kể các lệnh jump): PC <- PC + 4

## 1. Thiết Kế Datapath
Datapath: gồm các thành phần để xử lý dữ liệu và địa chỉ
### Các thành phần để thực hiện nhận lệnh
- Bộ đếm chương trình **PC**:
	- Thanh ghi 32-bit chứa địa chỉ của lệnh hiện tại
	- Địa chỉ khởi động = 0xBFC00000
- Bộ nhớ lệnh (Instruction Memory):
	- Chứa các lệnh của chương trình
	- Khi có địa chỉ lệnh từ PC đưa đến thì lệnh được đọc ra
- Bộ cộng (**Add**): Được sử dụng để tăng nội dung PC thêm 4 để trỏ tới lệnh kế tiếp
![[Pasted image 20230809213629.png]]
### Các thành phần thực hiện lệnh kiểu R
- Tập thanh ghi (**Registers**): Có 32 thanh ghi 32-bit, mỗi thanh ghi được xác định bởi số hiệu 5-bit
	- **Read register 1**, **Read register 2**: Các đầu vào để chọn thanh ghi cần đọc
	- **Write register**: Đầu vào để chọn thanh ghi cần ghi
	- **Read data 1**, **Read data 2**: Hai đầu ra dữ liệu đọc từ thanh ghi (32-bit)
	- **Write Data**: Đầu vào dữ liệu ghi vào thanh ghi (32-bit)
	- **RegWrite**: Tín hiệu điều khiển ghi dữ liệu vào thanh ghi
- **ALU** để thực hiện các phép toán số học / logic 
![[Pasted image 20230809214343.png]]
### Các thành phần thực hiện các lệnh lw/sw
![[Pasted image 20230809214917.png]]

# 5.3 Kỹ Thuật Đường Ống Lệnh và Song Song Mức Lệnh

