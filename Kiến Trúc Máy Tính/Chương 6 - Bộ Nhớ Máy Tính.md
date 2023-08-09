# 6.1 Tổng Quan Hệ Thống Nhớ
## 1. Các Đặc Trưng Của Bộ Nhớ
1. Vị trí:
- Bên trong CPU: các tập thanh ghi
- Bộ nhớ trong: Bộ nhớ chính, cache
- Bộ nhó ngoài: Các thiết bị lưu trữ
2. Dung lượng:
- Độ dài từ nhớ (tính bằng bit)
- Số lượng từ nhớ
3. Đơn vị truyền: **Từ nhớ** và **Khối nhớ**
4. Phương pháp truy nhập:
- Truy nhập tuần tự (băng từ)
- Truy nhập trực tiếp (các loại đĩa)
- Truy nhập ngẫu nhiên (bộ nhớ bán dẫn)
- Truy nhập liên kết (cache)
5. Hiệu năng (Performance)
- Thời gian truy nhập
- Chu kỳ nhớ
- Tốc độ truyền
6. Kiểu vật lý
- Bộ nhớ bán dẫn
- Bộ nhớ từ
- Bộ nhớ quang
7. Các đặc tính vật lý
8. Tổ chức
## 2. Phân cấp bộ nhớ
![[Pasted image 20230809233230.png]]
![[Pasted image 20230809234407.png]]
### Nguyên Lý Cục Bộ Hóa Tham Chiếu Bộ Nhớ
Trong một khoảng thời gian đủ nhỏ CPU thường chỉ tham chiếu các thông tin trong một khối nhớ cục bộ
# 6.2 Bộ Nhớ Chính
## 1. Bộ nhớ bán dẫn
![[Pasted image 20230809235012.png]]
## 2. Các đặc trưng cơ bản của bộ nhớ chính
- Chứa các chương trình đang thực hiện và các dữ liệu đang được sử dụng
- Tồn tại trên mọi hệ thống máy tính
- Bao gồm các ngăn nhớ được đánh địa chỉ trực tiếp bởi CPU
- Dung lượng của bộ nhớ chính nhỏ hơn không gian địa chỉ bộ nhớ mà CPU quản lý
- Việc quản lý logic bộ nhớ chính tùy thuộc vào hệ điều hành
### Tổ chức bộ nhớ đan xen (interleaved memory)
- Độ rộng của bus dữ liệu để trao đổi với bộ nhớ: m = 8, 16, 32, 64, 128 ... bit
- Các ngăn nhớ được tổ chức theo byte --> Tổ chức bộ nhớ vật lý khác nhau
# 6.3 Bộ Nhớ Đệm (Cache)
## 1. Nguyên tắc chung của Cache
- Cache có tốc độ nhanh hơn bộ nhớ chính
- Cache được đặt giữa CPU và bộ nhớ chính nhằm tăng tốc độ CPU truy cập bộ nhớ
- Cache có thể được đặt trên chip CPU
![[Pasted image 20230810000812.png]]
### Cấu trúc chung của cache / bộ nhớ chính
![[Pasted image 20230810000920.png]]
- Bộ nhớ chính có 2^N byte nhớ
- Bộ nhớ chính và cache được chia thành các khối có kích thước bằng nhau
	- Bộ nhớ chính: B0, B1, B2, ... B(q-1) (p Blocks)
	- Bộ nhớ cache: L0, L1, L2, ...., L(m-1) (m Lines)
	- Kích thước của Block (Line) = 8, 16, 32, 64, 128 byte
- Mỗi Line trong cache có một thẻ nhớ (Tag) được gắn vào.
- Một số Block của bộ nhớ chính được nạp vào các Line của cache
- Nội dung Tag (thẻ nhớ) cho biết Block nào của bộ nhớ chính hiện đang được chứa ở Line đó
- Nội dung Tag được cập nhật mỗi khi Block từ bộ nhớ chính nạp vào Line đó
- Khi CPU truy nhập (đọc / ghi) một từ nhớ, có hai khả năng xảy ra:
	- Từ nhớ đó có trong cache (cache hit)
	- Từ nhớ đó không có trong cache (cache miss)
## 2. Các phương pháp ánh xạ
- Ánh xạ trực tiếp (Direct Mapping)
- Ánh xạ liên kết toàn phần (Fully associative mapping)
- Ánh xạ liên kết tập hợp (Set associative mapping)
### Ánh xạ trực tiếp (Direct Mapping)
- Mỗi Block của bộ nhớ chính chỉ có thể được nạp vào một Line của cache
- Tổng quát: Bj chỉ có thể nạp vào L(j mod m) (m là số Line của cache)
![[Pasted image 20230810001856.png]]
- Địa chỉ N bit của bộ nhớ chính chia thành ba trường:
	- **Trường Word**: gồm W bit xác định một từ nhớ trong Block hay Line: 2^W = kích thước của Block hay Line
	- **Trường Line**: gồm L bit xác định một trong số các Line trong cache: 2^L = số Line trong cache = m
	- **Trường Tag**: gồm T bit: T = N - (W + L)
- Mỗi thẻ nhớ (Tag) của một Line chứa được T bit
- Khi Block từ bộ nhớ chính được nạp vào Line của cache thì Tag ở đó được cập nhật giá trị là **T bit địa chỉ bên trái** của Block đó
- Khi CPU muốn truy nhập một từ nhớ thì nó **phát ra một địa chỉ N bit cụ thể**
	- Nhờ vào giá trị L bit của trường Line sẽ tìm ra Line tương ứng
	- Đọc nội dung Tag ở Line đó (T bit), rồi so sánh với T bit bên trái của địa chỉ vừa phát ra
		- Giống nhau: cache hit
		- Khác nhau cache miss
### Ánh xạ liên kết toàn phần
- Mỗi Block có thể nạp vào bất kỳ Line nào của cache
- Địa chỉ của bộ nhớ chính chia thành hai trường:
	- Trường Word
	- Trường Tag dùng để xác định Block của bộ nhớ chính
- Tag xác định Block đang nằm ở Line đó
![[Pasted image 20230810003322.png]]
- Mỗi thẻ nhớ (Tag) của một Line chứa được T bit
- Khi Block từ bộ nhớ chính được nạp vào Line của cache thì Tag ở đó được cập nhật giá trị là T bit địa chỉ bên trái của Block đó
- Khi CPU muốn truy nhập một từ nhớ thì nó phát ra một địa chỉ N bit cụ thể
	- So sánh T bit bên trái của địa chỉ vừa phát ra với lần lượt nội dung của các Tag trong cache
		- Nếu gặp giá trị bằng nhau: cache hit
		- Nếu không có giá trị bằng nhau: cache miss
### Ánh xạ liên kết tập hợp
- Dung hòa cho hai phương pháp trên
- Cache được chia thành các Tập (Set)
- Mỗi một Set chứa một số Line
- Ánh xạ theo nguyên tắc:
	- B0 -> S0
	- B1 -> S1
	- B2 -> S2
	- ......
![[Pasted image 20230810003709.png]]
- Kích thước Block = 2^W Word
- Trường Set có S bit dùng để xác định một trong số các Set trong cache. 2^S = Số Set trong cache
- Trường Tag có T bit: T = N - (W + S)
- Khi CPU muốn truy nhập một từ nhớ thì nó phát ra một địa chỉ N bit cụ thể:
	- Nhờ vào giá trị S bit của trường Set se tìm ra Set tương ứng
	- So sánh T bit bên trái của địa chỉ vừa phát ra với lần lượt nội dung của các Tag trong Set đó
		- Nếu gặp giá trị bằng nhau: cache hit xảy ra ở Line tương ứng
		- Nếu không có giá trị nào bằng nhau: cache miss
- Tổng quát cho cả 2 phương pháp trên
- Thông dụng với: 2, 4, 8, 16 Lines / Set
# 6.4 Bộ Nhớ Ngoài
