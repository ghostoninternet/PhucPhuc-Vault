# Nội Dung
## 1. Đặt Vấn Đề
## 2. An Toàn Dữ Liệu
## 3. Toàn Vẹn Dữ Liệu
## 4. Giao dịch, điều khiển đồng thời

### 6.1 Đặt Vấn Đề

### 6.2 An Toàn Dữ Liệu
>**Định nghĩa:** Tính an toàn dữ liệu là sự bảo vệ dữ liệu là sự bảo vệ dữ liệu trong CSDL chống lại những truy nhập, sửa đổi hay phá hủy bất hợp pháp.
![[Triangle Permission.png]]
### 6.2.1 Các quyền truy nhập của người sử dụng
>**Đối với người khai thác CSDL:**
>	**- Quyền đọc dữ liệu:** đọc một phần hay toàn bộ dữ liệu
>	**- Quyền cập nhật dữ liệu:** sửa đổi nhưng không được xóa
>	**- Quyền xóa dữ liệu:** được phép xóa
>	**- Quyền bổ sung dữ liệu:** được phép thêm nhưng không được thay đổi
>
>**Đối với người quản trị CSDL:**
>	**- Quyền tạo chỉ dẫn:** trên các quan hệ CSDL
>	**- Quyền thay đổi sơ đồ CSDL:** thêm hay xóa thuộc tính của các quan hệ trong CSDL
>	**- Quyền loại bỏ quan hệ:** trong CSDL
>	**- Quyền quản lý tài nguyên:** được phép thêm các quan hệ mới vào CSDL
### Trách nhiệm của người quản trị hệ thông
