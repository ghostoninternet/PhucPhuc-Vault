# Nội Dung
## 1. Đặt Vấn Đề
## 2. An Toàn Dữ Liệu
## 3. Toàn Vẹn Dữ Liệu
## 4. Giao dịch, điều khiển đồng thời

### 6.1 ĐẶT VẤN ĐỀ

### 6.2 AN TOÀN DỮ LIỆU
>**Định nghĩa:** Tính an toàn dữ liệu là sự bảo vệ dữ liệu là sự bảo vệ dữ liệu trong CSDL chống lại những truy nhập, sửa đổi hay phá hủy bất hợp pháp.
![[Triangle Permission.png]]
### 6.2.1 Các Quyền Truy Nhập Của Người Sử Dụng
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
### Trách nhiệm của người quản trị hệ thống
>Để có thể phân biệt được người sử dụng trong hệ CSDL, người quản trị hệ thống phải có trách nhiệm: **phân quyền người sử dụng** và **xác minh người sử dụng**.
>**Xác minh người sử dụng**:
>	- Kỹ thuật dùng tài khoản
>	- Kỹ thuật sử dụng các hàm kiểm tra người sử dụng
>	- Dùng thẻ, nhận dạng

### Các câu lệnh an toàn sử dụng dữ liệu trong SQL
### 1. Câu lệnh tạo khung nhìn
`CREATE VIEW <Tên khung nhìn> [các cột] AS <Câu truy vấn>`
### 2. Câu lệnh phân quyền cho NSD
`GRANT <Danh sách thao tác> ON <Đối tượng> TO <Danh sách người dùng> [WITH GRANT OPTION]`
`<Dánh sách thao tác>`: có thể bao gồm một hay nhiều thao tác được liệt kê dưới đây
	- Insert: được thêm nhưng không được thay đổi
	- Update: sửa đổi nhưng không được xóa
	- Delete: xóa dữ liệu
	- Select: tìm kiếm
	- Create: tạo lập các quan hệ mới
	- Alter: thay đổi cấu trúc quan hệ
	- Drop: loại bỏ quan hệ
	- Read/Write: đọc và ghi
`<Đối tượng>`: bảng hoặc khung nhìn
`<Danh sách người dùng>`: một người hay một nhóm nhóm hay một danh sách người sử dụng. Từ khóa public được dùng thay thế cho mọi người sử dụng
`[WITH GRANT OPTION]`: Nếu dùng từ khóa này trong câu lệnh phân quyền thì người dùng xuất hiện trong `<Danh sách người dùng>` có quyền được lan truyền các quyền vừa được tuyên bố cho những người dùng khác.
### 3. Câu lệnh thu hồi quyền của NSD
`REVOKE <Danh sách thao tác> ON <Đối tượng> FROM <Danh sách người dùng> [RESTRICT/CASCADE]`
`RESTRICT`: chỉ hủy bỏ quyền của những người có trong danh sách, quyền đã được lan truyền cho người khác không bị thu hồi
`CASCADE`: hủy bỏ quyền trong danh sách, đồng thời kéo theo hủy bỏ quyền mà người dùng đó đã luân chuyển cho những người khác

### 6.3 TOÀN VẸN DỮ LIỆU
>**Định nghĩa:** Tính toàn vẹn dữ liệu là sự bảo vệ dữ liệu trong CSDL chống lại những sự sửa đổi, phá hủy vô căn cứ để đảm bảo tính đúng đắn và chính xác của dữ liệu
### Các ràng buộc toàn vẹn trong SQL
`CREATE ASSERTION <Tên khẳng định> CHECK <Vị từ>`
![[Example_of_Asertion.png]]
>Đặc điểm của **trigger**:
>	- Một trigger có thể làm nhiều công việc, có thể được kích hoạt bởi nhiều sự kiện
>	- Trigger không thể được tạo ra trên bảng tạm hoặc bảng hệ thống
>	- Trigger chỉ có thể được kích hoạt tự động bởi các sự kiện mà không thể chạy thủ công được
>	- Có thể áp dụng cho view
>	- Khi trigger được kích hoạt
>		  * Dữ liệu mới được **insert** sẽ được chứa trong bảng **inserted**
>		  * Dữ liệu mới được **delete** sẽ được chứa trong bảng **deleted**
>		  * Đây là hai bảng tạm nằm trên bộ nhớ, và chỉ có giá trị bên trong trigger
>Cú pháp của **trigger**:
```SQL
CREATE [OR REPLACE] TRIGGER <Trigge_name>
	{BEFORE | AFTER | INSTEAD OF}
	{INSERT | DELETE | UPDATE [OF <attribute_name>]}
	ON <table_name>
	REFERENCING {NEW | OLD} {ROW | TABLE} AS <name>
	[FOR EACH ROW]
	[WHEN (<condition>)]
	BEGIN
		<trigger body goes here>
	END;
```
>`AFTER | BEFORE`: sử dụng cho table và view
>`INSTEAD OF`: chỉ sử dụng cho view
>`UPDATE OF <columns>`: sửa đổi một cột cụ thể
>Trigger ở mức row: được dùng bởi option `FOR EACH ROW`
![[Trigger_example_1.png]]
![[Trigger_example_2.png]]
