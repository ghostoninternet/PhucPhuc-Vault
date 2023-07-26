# Nội Dung:
## 1. Tổng quan về xử lý truy vấn
## 2. Tối ưu hóa các biểu thức đại số quan hệ

### 5.1. Tổng quan về xử lý truy vấn
>Xử lý một truy vấn bao gồm 3 bước chính:
>**Phân tích và Biên dịch câu truy vấn**: ngôn ngữ bậc cao --> ngôn ngữ biểu diễn dữ liệu bên trong, ngôn ngữ đại số quan hệ
>**Tối ưu hóa câu truy vấn**: Chọn ra một kế hoạch thực hiện câu truy vấn có chi phí thấp nhất
>**Thực hiện đánh giá truy vấn**: Từ output của bước 2, thực hiện các thao tác trên dữ liệu trong CSDL và đưa ra output của câu truy vấn đó.
![[Query Processing.png]]
### Tối Ưu Hóa Câu Truy Vấn
>**1. Tối ưu hóa đại số:** Biến đổi biểu thức ĐSQH đầu vào thành một biểu thức ĐSQH tương đương nhưng có thể **xử lý hiệu quả** và **ít tốn kém hơn**.
>**2. Đặc tả các thuật toán đặc biệt tiến hành thực thi các phép toán, chọn một chỉ dẫn cụ thể nào đó để sử dụng**.
>Các dữ liệu thống kê về CSDL sẽ giúp ta trong quá trình xem xét và lựa chọn. Chi phí ve
