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
![[Pasted image 20230809220040.png]]
![[Pasted image 20230809220052.png]]
### Các thành phần thực hiện lệnh Branch
![[Pasted image 20230809215448.png]]
![[Pasted image 20230809220000.png]]
### Hợp các thành phần cho các lệnh
- Datapath cho các lệnh thực hiện trong 1 chu kỳ
	- Mỗi phần tử của datapath chỉ có thể làm một chức năng trong mỗi chu kỳ
	- Do đó, cần tách rời bộ nhớ lệnh và bộ nhớ dữ liệu
- Sử dụng các bộ chọn kênh để chọn dữ liệu nguồn cho các lệnh khác nhau
![[Pasted image 20230809215636.png]]
![[Pasted image 20230809215648.png]]

## 2. Thiết Kế Control Unit
Đơn vị điều khiển có hai phần:
- Bộ điều khiển ALU
- Bộ điều khiển chính
ALU được sử dụng để:
- Load/Store: F = add (xác định địa chỉ bộ nhớ dữ liệu)
- Branch: F = subtract (so sánh)
- Các lệnh số học/logic: F phụ thuộc vào funct code
Bộ điều khiển ALU sử dụng mạch logic tổ hợp:
- Đầu vào: **2-bit ALUOp được tạo ra từ opcode của lệnh** và **6-bit của function code**
- Đầu ra: các tín hiệu điều khiển ALU (ALU Control) gồm 4 bit
![[Pasted image 20230809222316.png]]
![[Pasted image 20230809222410.png]]
![[Pasted image 20230809222451.png]]
![[Pasted image 20230809222509.png]]
![[Pasted image 20230809222520.png]]
![[Pasted image 20230809222536.png]]
![[Pasted image 20230809222545.png]]
![[Pasted image 20230809222631.png]]
# 5.3 Kỹ Thuật Đường Ống Lệnh và Song Song Mức Lệnh
- Kỹ thuật đường ống lệnh (Instruction Pipelining): Chia chu trình lệnh thành các công đoạn và cho phép thực hiện gối lệnh lên nhau (như dây chuyền lắp rắp)
- Bộ xử lý MIPS có 5 công đoạn:
	1. **IF**: Nhận lệnh từ bộ nhớ
	2. **ID**: Giải mã lệnh và đọc thanh ghi
	3. **EX**: Thực hiện thao tác hoặc tính toán địa chỉ
	4. **MEM**: Truy nhập toán hạng bộ nhớ
	5. **WB**: Ghi kết quả trả về thanh ghi
![[Pasted image 20230809223148.png]]
### Các mối trở ngại (HAZARD) của đường ống lệnh
- Hazard: Tình huống ngăn cản bắt đầu của lệnh tiếp theo ở chu kỳ tiếp theo
	- **Hazard cấu trúc**: do tài nguyên được yêu cầu đang bận
	- **Hazard dữ liệu**: cần phải đợi lệnh trước hoàn thành việc đọc / ghi dữ liệu
	- **Hazard điều khiển**: do rẽ nhánh gây ra
### Hazard cấu trúc
Xung đột khi sử dụng tài nguyên
Trong đường ống của MIPS với một bộ nhớ dùng chung
	- Lệnh Load / Store yêu cầu truy cập dữ liệu
	- Nhận lệnh cần trì hoãn cho chu kỳ đó
Bởi vậy, datapath kiểu đường ống yêu cầu bộ nhớ lệnh và bộ nhớ dữ liệu tách rời (hoặc cache lệnh / cache dữ liệu tách rời)
### Hazard dữ liệu
Lệnh phụ thuộc vào việc hoàn thành truy cập dữ liệu của lệnh trước đó
![[Pasted image 20230809223937.png]]
### Fowarding (Gửi vượt trước)
Sử dụng kết quả ngay sau khi nó được tính
	- Không đợi đến khi kết quả được lưu đến thanh ghi
	- Yêu cầu có đường kết nối thêm trong datapath
![[Pasted image 20230809224147.png]]
### Hazard dữ liệu với lệnh load
Không phải luôn luôn có thể tránh trì hoãn bằng cách forwarding
	- Nếu giá trị chưa được tính khi cần thiết
	- Không thể chuyển ngược thời gian
	- Cần chèn bước trì hoãn (stall hay bubble)
![[Pasted image 20230809224425.png]]
### Lập lịch mã để tránh trì hoãn
Thay đổi trình tự mã để tránh sử dụng kết quả load ở lệnh tiếp theo
![[Pasted image 20230809224607.png]]
### Hazard điều khiển
Rẽ nhánh xác định luồng điều khiển
	- 