# 7.1 Tổng Quan Về Hệ Thống Vào-Ra
- Chức năng: Trao đổi thông tin giữa máy tính với bên ngoài
- Các thao tác cơ bản: Vào dữ liệu (Input) + Ra dữ liệu (Output)
- Các thành phần chính: Các thiết bị vào ra + Các mô-đun vào ra
![[Pasted image 20230810073802.png|500x500]]
### Đặc điểm của hệ thống vào-ra
- Tồn tại đa dạng các thiết bị vào-ra khác nhau về:
	- Nguyên tắc hoạt động
	- Tốc độ
	- Khuôn dạng dữ liệu
- Tất cả các thiết bị vào-ra đều chậm hơn so với CPU và Ram --> Cần có các mô-đun vào ra để nối ghép các thiết bị với CPU và bộ nhớ chính
### Thiết bị vào-ra
- Còn gọi là **thiết bị ngoại vi (peripherals)**
- Chức năng: Chuyển đổi dữ liệu giữa bên trong và bên ngoài máy tính
- Phân loại:
	- Thiết bị vào (Input Devices)
	- Thiết bị ra (Output Devices)
	- Thiết bị lưu trữ (Storage Devices)
	- Thiết bị truyền thông (Communication Devices)
- Giao tiếp: Người - Máy, Máy - Người
### Mô-đun vào-ra
- Chức năng: 
	- Điều khiển và định thời
	- Trao đổi thông tin với CPU hoặc bộ nhớ chính
	- Trao đổi thông tin với thiết bị vào-ra
	- Đệm giữa bên trong máy tính với thiết bị vào-ra
	- Phát hiện lỗi của thiết bị vào-ra
### Địa chỉ hóa cổng vào-ra (IO addressing)
- Hầu hết các bộ xử lý chỉ có một không gian địa chỉ chung cho cả các ngăn nhớ và các cổng vào-ra (Ví dụ: MIPS)
- Một số bộ xử lý có hai không gian địa chỉ tách biệt (Ví dụ: Intel x86):
	- Không gian địa chỉ bộ nhớ
	- Không gian địa chỉ vào-ra
### Các phương pháp địa chỉ hóa cổng vào-ra
**Vào ra theo bản đồ bộ nhớ (Memory mapped IO)
- Cổng vào-ra được đánh địa chỉ theo không gian địa chỉ bộ nhớ
- CPU coi cổng vào-ra như ngăn nhớ
- Lập trình trao đổi dữ liệu với cổng vào-ra bằng ca
# 7.2 Các Phương Pháp Điều Khiển Vào-Ra
# 7.3 Nối Ghép Thiết Bị Vào-Ra
