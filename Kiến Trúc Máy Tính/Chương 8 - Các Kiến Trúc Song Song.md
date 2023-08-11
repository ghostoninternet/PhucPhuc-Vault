# 8.1 Phân Loại Kiến Trúc Máy Tính
Phân loại kiến trúc máy tính:
- **SISD - Single Instruction Stream, Single Data Stream**
![[Pasted image 20230810142701.png]]
- **SIMD - Single Instruction Stream, Multiple Data Stream**
![[Pasted image 20230810142748.png]]
- Đơn dòng lệnh điều khiển đồng thời các đơn vị xử lý PUs
- Mỗi đơn vị xử lý có một bộ nhớ dữ liệu riêng LM (local memory)
- Mỗi lệnh được thực hiện trên một tập các dữ liệu khác nhau
- Các mô hình SIMD: 
	- Vector Computer
	- Array Processor
- **MISD - Multiple Instruction Stream, Single Data Stream**
- Một luồng dữ liệu cùng được truyền đến một tập các bộ xử lý
- Mỗi bộ xử lý thực hiện một dãy lệnh khác nhau
- **MIMD - Multiple Instruction Stream, Multiple Data Stream**
- Tập các bộ xử ký
- Các bộ xử lý đồng thời thực hiện các dãy lệnh khác nhau trên các dữ liệu khác nhau
- Các mô hình MIMD:
	- Multiprocessors (Shared Memory)
		![[Pasted image 20230810143529.png]]
	- Multicomputers (Distributed Memory)
		![[Pasted image 20230810143546.png]]
# 8.2 Đa Xử Lý Bộ Nhớ Dùng Chung
## Hệ thống đa xử lý đối xứng (SMP - Symmetric Multiprocessors)
![[Pasted image 20230810144509.png]]
![[Pasted image 20230810144519.png]]
- Một máy tính có n >= 2 bộ xử lý giống nhau
- Các bộ xử lý dùng chung bộ nhớ và hệ thống vào-ra
- Thời gian truy cập bộ nhớ là bằng nhau với các bộ xử lý
- Các bộ xử lý có thể thực hiện chức năng giống nhau
- Hệ thống được điều khiển bởi một hệ điều hành phân tán
- Hiệu năng: Các công việc có thể thực hiện song song
- Khả năng chịu lỗi
## Hệ thống đa xử lý không đối xứng (NUMA - Non - Uniform Memory Access)
- Có một không gian địa chỉ chung cho tất cả CPU
- Mỗi CPU có thể truy cập từ xa sang bộ nhớ của CPU khác
- Truy nhập bộ nhớ từ xa chậm hơn truy nhập bộ nhớ cục bộ
![[Pasted image 20230810145309.png]]
## Bộ xử lý đa lõi (Multicore Processors)
- Thay đổi của bộ xử lý:
	- Tuần tự
	- Pipeline
	- Siêu Vô Hướng
	- Đa luồng
	- Đa lõi: nhiều CPU trên một chip
![[Pasted image 20230810145508.png]]

# 8.3 Đa Xử Lý Bộ Nhớ Phân Tán
# 8.4 Bộ Xử Lý Đồ Họa Đa Dụng
