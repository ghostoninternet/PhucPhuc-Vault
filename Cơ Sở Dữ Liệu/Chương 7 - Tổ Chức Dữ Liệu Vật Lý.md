# Nội dung:
## 1. Mô hình tổ chức bộ nhớ
## 2. Tổ chức tệp đống
## 3. Tổ chức tệp băm
## 4. Tổ chức tệp chỉ dẫn
## 5. Cây cân bằng

### 7.1. MÔ HÌNH TỔ CHỨC BỘ NHỚ
>Phương tiện nhớ của máy tính hình thành một phân cấp bộ nhớ bao gồm 2 loại chính:
>**- Bộ nhớ sơ cấp:**
>		- Bộ nhớ chính, bộ nhớ đệm cache
>		- Truy cập dữ liệu nhanh nhưng bị giới hạn dung lượng
>**- Bộ nhớ thứ cấp:** 
>		- Đĩa từ, đĩa quang, băng từ
>		- Dung lượng lớn
>		- Truy cập dữ liệu chậm

### 7.2 TỔ CHỨC TỆP ĐỐNG (HEAP FILE)
> **Tổ chức dữ liệu**: Bản ghi lưu trữ kế tiếp trong các khối, không tuân theo một thứ tự đặc biệt nào
> **Các thao tác**:
> 	- Tìm kiếm một bản ghi: quét toàn bộ tệp
> 	- Thêm một bản ghi: thêm bản ghi mới vào sau bản ghi cuối cùng
> 	- Xóa một bản ghi: thao tác xóa bao hàm thao tác tìm kiếm. Nếu có bản ghi cần xóa nó sẽ được đánh dấu là xóa => hệ thống cần tổ chức lại đĩa định kỳ
> 	- Sửa một bản ghi: tìm bản ghi rồi sửa một hay nhiều trường
![[Heap-file-example.png]]

### 7.3 TỔ CHỨC TỆP BĂM (HASHED FILES)
>**Tổ chức dữ liệu**:
>- Phân chia các bản ghi vào các cụm
>- Mỗi cụm gồm một hoặc nhiều khối
>- Mỗi khối chứa số lượng bản ghi cố định
>- Tổ chức lưu trữ dữ liệu trong mỗi cụm áp dụng theo tổ chức đống
>**Tiêu chí chọn hàm băm:** phân bố các bản ghi tương đối đồng đều theo các cụm
>![[Hashed-file-example-1.png]]
>![[Hashed-file-example-2.png]]
>**Các thao tác**:
>**- Tìm kiếm một bản ghi:** Tính h(x) --> cụm chứa bản ghi --> tìm theo tổ chức đống
>**- Thêm một bản ghi:** Thêm một bản ghi có giá trị khóa là *x*.
>	- Nếu trong tệp đã có bản ghi có trùng khóa *x* => bản ghi mới sai
>	- Nếu không có bản ghi trùng khóa, bản ghi mới được thêm vào khối còn chỗ trống đầu tiên trong cụm, nếu hết chỗ thì tạo khối mới
>**- Xóa một bản ghi:** Tìm rồi xóa
>**- Sửa đổi bản ghi:**
>	- Nếu trường cần sửa có tham gia vào trong khóa thì việc sửa sẽ là loại bỏ bản ghi này và thêm mới một bản ghi (bản ghi có thể thuộc vào một cụm khác)
>	- Nếu trường cần sửa không thuộc khóa: tìm kiếm rồi sửa. 

### 7.4. TỔ CHỨC TỆP CHỈ DẪN (INDEXED FILES)
>Giả sử giá trị các khóa của bản ghi được **sắp xếp tăng dần**. **Tệp chỉ dẫn** được tạo bằng cách chọn các **giá trị khóa trong các bản ghi**.
>**Tệp chỉ dẫn** bao gồm các **cặp (k, d)**, trong đó **k** là **giá trị khóa** của **bản ghi đầu tiên**, **d** là **địa chỉ của khối** 
>![[indexed-files.png]]
>**Các thao tác**:
>**-TÌm kiếm:**
>**-Thêm một bản ghi:**
>**-Xóa một bản ghi:**
>**-Sửa một bản ghi:**

### 7.5 CÂY CÂN BẰNG (BALANCED-TREES)
>B-tree được tổ chức theo cấp m có các tính chất sau:
>- Gốc của cây hoặc là một nút lá hoặc là ít nhất có hai con
>- Mỗi nút (trừ nút gốc và nút lá) có từ [m/2] đến m con
>- Mỗi đường đi từ nút gốc đến bất kỳ nút lá nào đều có độ dài như nhau
>![[B-tree.png]]
>**Các thao tác:**
>**- TÌm kiếm một bản ghi:**
>**- Thêm một bản ghi:**
>**- Loại bỏ một bản ghi:**

## KẾT LUẬN
#### Tổ chức tệp chỉ dẫn:
- Được áp dụng phổ biến
- Với các ứng dụng yêu cầu cả xử lý tuần tự và truy nhập trực tiếp đến các bản ghi
- Hiện năng sẽ giảm khi kích thước tệp tăng => Chỉ dẫn B-cây
#### Tổ chức băm:
- Dựa trên một hàm băm, cho phép tìm thấy địa chỉ khoản mục dữ liệu một cách trực tiếp
- Hàm băm tốt ? Phân bố các bản ghi đồng đều trong cụm