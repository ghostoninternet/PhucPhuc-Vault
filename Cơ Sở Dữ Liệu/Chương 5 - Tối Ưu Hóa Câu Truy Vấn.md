# Nội Dung:
## 1. Tổng quan về xử lý truy vấn
## 2. Tối ưu hóa các biểu thức đại số quan hệ

### 5.1. TỔNG QUAN VỀ XỬ LÝ TRUY VẤN
>Xử lý một truy vấn bao gồm 3 bước chính:
>**Phân tích và Biên dịch câu truy vấn**: ngôn ngữ bậc cao --> ngôn ngữ biểu diễn dữ liệu bên trong, ngôn ngữ đại số quan hệ
>**Tối ưu hóa câu truy vấn**: Chọn ra một kế hoạch thực hiện câu truy vấn có chi phí thấp nhất
>**Thực hiện đánh giá truy vấn**: Từ output của bước 2, thực hiện các thao tác trên dữ liệu trong CSDL và đưa ra output của câu truy vấn đó.
![[Query Processing.png]]
### Tối Ưu Hóa Câu Truy Vấn
>**1. Tối ưu hóa đại số:** Biến đổi biểu thức ĐSQH đầu vào thành một biểu thức ĐSQH tương đương nhưng có thể **xử lý hiệu quả** và **ít tốn kém hơn**.
>**2. Đặc tả các thuật toán đặc biệt tiến hành thực thi các phép toán, chọn một chỉ dẫn cụ thể nào đó để sử dụng**.
>Các dữ liệu thống kê về CSDL sẽ giúp ta trong quá trình xem xét và lựa chọn. Chi phí cho việc thực hiện một câu truy vấn được đo bởi chi phí sử dụng tài nguyên như: truy cập ổ đĩa, thời gian CPU chạy để xử lý câu truy vấn.
### Đánh giá biểu thức ĐSQH
Có 2 hướng tiếp cận để đánh giá biểu thức ĐSQH:
> **Vật chất hóa (Materialize)**: 
> 	- Lần lượt đánh giá các phép toán.
> 	- Kết quả của việc đánh giá mỗi phép toán sẽ được lưu trong một quan hệ trung gian tạm thời để sử dụng làm đầu vào cho các phép toán tiếp theo.
> 	- **Điểm bất lợi**: cần các quan hệ trung gian (ghi ra ổ đĩa có chi phí khá lớn)
> **Đường ống (Pipeline):** 
> 	- Kết hợp một vài phép toán quan hệ vào đường ống của các phép toán
> 	- Trong đường ống thì kết quả của một phép toán được chuyển trực tiếp cho phép toán tiếp theo mà không cần phải lưu trong quan hệ trung gian.
> 	- Hạn chế nhược điểm của cách 1 nhưng có vài trường hợp không dùng được **đường ống**.

### 5.2 TỐI ƯU HÓA CÁC BIỂU THỨC ĐSQH
>Mục tiêu là tổ chức lại **trình tự thực hiện các phép toán** trong biểu thức để **giảm chi phí thực hiện đánh giá** biểu thức đó
>Trong quá trình tối ưu hóa, ta biểu diễn một biểu thức ĐSQH dưới dạng một cây toán tử. Trong cây thì:
>		**1.** Các **nút lá** là các **quan hệ có mặt** trong biểu thức
>		**2.** Các **nút trong** là các **phép toán** trong biểu thức
### Các Chiến Lược Tối Ưu Tổng Quát
>**1.** Đẩy **phép chọn** và **phép chiếu** xuống thực hiện sớm nhất có thể
>
>**2.** Nhóm dãy các **phép chọn** và **chiếu**: Sử dụng chiến lược này
>	nếu như có một dãy các **phép chọn** hoặc dãy các **phép chiếu** trên **cùng một quan hệ**.
>
>**3.** Kết hợp **phép chọn** và **tích Đề-các** thành **phép kết nối:** Nếu
>	kết quả của một **phép tích Đề-các** là đối số của 1 **phép chọn** có điều kiện là **phép so sánh** giữa các thuộc tính trên 2 quan hệ tham gia tích Đề-các thì ta nên **kết hợp 2 phép toán thành phép kết nối**.
>
>**4.** Tìm các **biểu thức con chung** trong biểu thức **ĐSQH** để đánh
>	giá chỉ một lần
>
>**5.** Xác định các phép toán **có thể đưa vào đường ống** và thực
>	hiện đánh giá chúng theo đường ống.
>
>**6.** Xử lý các tệp dữ liệu trước khi tiến hành tính toán
>
>**7.** Ước lượng chi phí và lựa chọn thứ tự thực hiện
### Các phép biến đổi tương đương biểu thức ĐSQH
>Hai biểu thức ĐSQH **E1** và **E2** là tương đương nếu chúng cho cùng một kết quả khi áp dụng trên cùng một tập các quan hệ
>Các ký hiệu:
>		**- E1, E2, E3, ...** là các biểu thức ĐSQH
>		**- F1, F2, F3, ...** là các điều kiện chọn hoặc là các điều kiện kết nối
>		**- X1, X2, ... Y, Z, U1, U2, ...** là các tập thuộc tính
![[Rule no1.png]]
![[Rule no234.png]]
![[Rule no56.png]]
![[Rule no78910.png]]
