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
- Lập trình trao đổi dữ liệu với cổng vào-ra bằng các lệnh truy nhập dữ liệu bộ nhớ
- Có thể thực hiện trên mọi hệ thống
**Vào ra riêng biệt (Isolated IO)**
- Cổng vào-ra được đánh địa chỉ theo không gian địa chỉ vào-ra riêng
- Lập trình trao đổi dữ liệu với cổng vào-ra bằng các lệnh vào-ra chuyên dụng
# 7.2 Các Phương Pháp Điều Khiển Vào-Ra
## 1. Vào-ra bằng chương trình
- Nguyên tác chung:
	- CPU điều khiển trực tiếp vào-ra bằng chương trình --> Cần phải lập trình vào-ra để trao đổi dữ liệu giữa CPU với mô-đun vào ra
	- CPU nhanh hơn thiết bị ngoại vi rất nhiều lần, vì vậy trước khi thực hiện lệnh vào-ra, chương trình cần đọc và kiểm tra trạng thái sẵn sàng của mô-đun vào-ra
![[Pasted image 20230810080142.png|400x600]]
### Các tín hiệu điều khiển vào-ra
- Tín hiệu điều khiển (Control): kích hoạt thiết bị vào-ra
- Tín hiệu kiểm tra (Test): kiểm tra trạng thái của mô-đun vào-ra và thiết bị vào-ra
- Tín hiệu điều khiển đọc (Read): yêu cầu mô-đun vào-ra nhận dữ liệu từ thiết bị vào-ra và đưa vào bộ đệm dữ liệu, rồi CPU nhận dữ liệu đó
- Tín hiệu điều khiển ghi (Write): yêu cầu mô-đun vào-ra lấy dữ liệu trên bus dữ liệu đưa đến bộ đệm dữ liệu rồi chuyển ra thiết bị vào-ra
### Các lệnh vào ra
Memory Mapped IO: sử dụng các lệnh trao đổi dữ liệu với bộ nhớ
Isolated IO: sử dụng các lệnh vào-ra chuyên dụng (IN, OUT)
### Đặc điểm
- Vào-ra do ý muốn của người lập trình
- CPU trực tiếp điều khiển trao đổi dữ liệu giữa CPU với mô-đun vào-ra
- CPU đợi trạng thái sẵn sàng của mô-đun vào-ra (thông qua vòng lặp) --> tiêu tốn nhiều thời gian của CPU
## 2. Vào-ra điều khiển bằng ngắt
- Nguyên tắc chung:
	- CPU không phải đợi trạng thái sẵn sàng của mô-đun vào-ra, CPU thực hiện một chương trình nào đó
	- Khi mô-đun vào-ra sẵn sàng thì nó phát tín hiệu ngắt CPU
	- CPU thực hiện chương trình con xử lý ngắt vào-ra tương ứng để trao đổi dữ liệu
	- CPU trở lại tiếp tục thực hiện chương trình đang bị ngắt
![[Pasted image 20230810081942.png]]
### Hoạt động vào dữ liệu
**Nhìn từ mô-đun vào-ra**: 
- Mô-đun vào-ra nhận tín hiệu điều khiển đọc từ CPU
- Mô-đun vào-ra nhận dữ liệu từ thiết bị ngoại vi, trong khi đó CPU làm việc khác
- Khi đã có dữ liệu --> Mô-đun vào-ra phát tín hiệu ngắt CPU
- CPU yêu cầu dữ liệu
- Mô-đun vào ra chuyển dữ liệu đến CPU
**Nhìn từ CPU**:
- Phát tín hiệu điều khiển đọc
- Làm việc khác
- Cuối mỗi chu trình lệnh, kiểm tra tín hiệu yêu cầu ngắt
- Nếu bị ngắt:
	- Cất ngữ cảnh
	- Thực hiện chương trình con xử lý ngắt để vào dữ liệu
	- Khôi phục ngữ cảnh của chương trình đang thực hiện
### Đặc điểm của vào-ra điều khiển bằng ngắt
- Có sự kết hợp giữa phần cứng và phần mềm
	- Phần cứng: gây ngắt CPU
	- Phần mềm: trao đổi dữ liệu giữa CPU với mô-đun vào-ra
- CPU trực tiếp điều khiển vào-ra
- CPU không phải đợi mô-đun vào-ra, do đó hiệu quả sử dụng CPU tốt hơn
## 3. DMA (Direct Memory Access)
- Vào-ra bằng chương trình và bằng ngắt do CPU trực tiếp điều khiển --> Chiếm thời gian của CPU
- Để khắc phục dùng kỹ thuật DMA: Sử dụng mô-đun điều khiển vào-ra chuyên dụng, gọi là DMAC (Controller), điều khiển trao đổi dữ liệu giữa mô-đun vào-ra với bộ nhớ chính
![[Pasted image 20230810083224.png]]
### Các thành phần của DMAC
- Thanh ghi dữ liệu: chứa dữ liệu trao đổi
- Thanh ghi địa chỉ: chứa địa chỉ ngăn nhớ dữ liệu
- Bộ đếm dữ liệu: chứa số từ dữ liệu cần trao đổi
- Logic điều khiển: điều khiển hoạt động của DMAC
### Hoạt Động DMA
- CPU "nói" cho DMAC
	- Vào hay Ra dữ liệu
	- Địa chỉ thiết bị vào-ra (cổng vào-ra tương ứng)
	- Địa chỉ đầu của mảng nhớ chứa dữ liệu -> nạp vào thanh ghi địa chỉ
	- Số từ dữ liệu cần truyền -> nạp vào bộ đếm dữ liệu
- CPU làm việc khác
- DMAC điều khiển trao đổi dữ liệu
- Sau khi truyền được một từ dữ liệu thì:
	- Nội dung thanh ghi địa chỉ tăng
	- Nội dung bộ đếm dữ liệu giảm
- Khi bộ đếm dữ liệu = 0, DMAC gửi tín hiệu ngắt CPU để báo kết thúc DMA
### Các kiểu thực hiện DMA
- **DMA truyền theo khối (Block-transer DMA)**: DMAC sử dụng bus để truyền xong cả khối dữ liệu
- **DMA lấy chu kỳ (Cycle Stealing DMA)**: DMAC cưỡng bức CPU treo tạm thời từng chu kỳ bus, DMAC chiếm bus thực hiện truyền một từ dữ liệu.
- **DMA trong suốt (Transparent DMA)**: DMAC nhận biết những chu kỳ nào CPU không sử dụng bus thì chiếm bus để trao đổi một từ dữ liệu.
### Đặc điểm của DMA
- CPU không tham gia trong quá trình trao đổi dữ liệu
- DMAC điều khiển trao đổi dữ liệu giữa bộ nhớ chính với mô-đun vào ra (hoàn toàn bằng phần cứng) --> Tốc độ nhanh
- Phù hợp với các yêu cầu trao đổi mảng dữ liệu có kích thước lớn
# 7.3 Nối Ghép Thiết Bị Vào-Ra
## 1. Các kiểu nối ghép vào-ra
### Nối ghép song song:
- Truyền nhiều bit song song
- Tốc độ nhanh
- Cần nhiều đường truyền dữ liệu
![[Pasted image 20230810084151.png]]
### Nối ghép nối tiếp
- Truyền lần lượt từng bit
- Cần có bộ chuyển đổi từ dữ liệu song song sang nối tiếp hoặc / và ngược lại
- Tốc độ chậm hơn
- Cần ít đường truyền dữ liệu
![[Pasted image 20230810084251.png]]
## 2. Các cấu hình nối ghép
- Điểm tới điểm (Point to Point): thông qua một cổng vào-ra nối ghép với một thiết bị
- Điểm tới đa điểm (Point to Multipoint): thông qua một cổng vào-ra cho phép nối ghép được với nhiều thiết bị
