## Object-Oriented Programming

### Các đặc điểm cơ bản của lập trình hướng đối tượng. Chúng được thể hiện như thế nào trong PHP
>Các đặc điểm cơ bản của lập trình hướng đối tượng bao gồm 4 đặc điểm:
>1. Trừu tượng hóa (Abstraction)
>2. Đóng gói (Encapsulation)
>3. Kế thừa (Inheritance)
>4. Đa hình (Polymorphism)
#### Trừu tượng hóa (Abstraction)
>Đây là việc định nghĩa ra một lớp cha chung, được gọi là abstract class hay interface class. Ở trong lớp cha chung này ta chỉ cần định nghĩa ra các property, các method mà không cần phải triển khai cụ thể các property hay method đó. 
>Khi lớp con mở rộng từ lớp cha chung đó, bên trong lớp con phải triển khai cụ thể cho các property, method mà nó đã kế thừa từ lớp cha.
>Lợi ích của tính trừu tượng hóa bao gồm:
>1. Tăng tính linh hoạt: Cho phép thay đổi và mở rộng chi tiết cụ thể của đối tượng mà không ảnh hưởng đến đối tượng khác.
>2. Giảm sự phức tạp: Tính trừu tượng hóa loại bỏ đi việc định nghĩa những property, method chung giữa nhiều lớp và đưa những property, method chung đó ra thành một lớp trừu tượng. Các lớp con chỉ cần mở rộng, kế thừa từ lớp trừu tượng đó. Điều này còn giúp ta có thể tập trung vào sự khác biệt giữa các lớp con.
>
>Sau đây là một ví dụ trong PHP:
```PHP
<?php
declare(strict_types=1);

abstract class SinhVien
{
    public string $NganhHoc;
    public int $SoNamHoc;
    public string $NgoaiNgu;

    public function setData($NganhHoc, $SoNamHoc, $NgoaiNgu)
    {
        $this->NganhHoc = $NganhHoc;
        $this->SoNamHoc = $SoNamHoc;
        $this->NgoaiNgu = $NgoaiNgu;
    }
}

class SV_ITE6 extends SinhVien
{
    public function __construct($NganhHoc, $SoNamHoc, $NgoaiNgu)
    {
        $this->setData($NganhHoc, $SoNamHoc, $NgoaiNgu);
    }
    public function viewData()
    {
        echo $this->NganhHoc;
        echo "<br>";
        echo $this->SoNamHoc;
        echo "<br>";
        echo $this->NgoaiNgu;
    }

}
$sinhvien1 = new SV_ITE6("CNTT Viet Nhat", 4, "Tieng Viet va Tieng Nhat");

$sinhvien1->viewData();
/* Output
CNTT Viet Nhat
4  
Tieng Viet va Tieng Nhat
*/
?>
```
#### Tính đa hình (Polymorphism)
>Thể hiện qua việc có thể định nghĩa một đặc tính, hoặc phương thức cho một loạt các đối tượng gần giống nhau. Nhưng khi thực hiện thì các đối tượng khác nhau sẽ có cách thực hiện khác nhau và cho ra kết quả khác nhau.
>Tính đa hình (polymorphism) trong lập trình hướng đối tượng cho phép các lớp con có thể viết lại (override) các thuộc tính hoặc phương thức từ lớp cha. Trong PHP:
>1. Các lớp con có thể viết lại hoặc mở rộng phương thức của lớp cha mà nó kế thừa.
>2. Các class cùng implement một interface nhưng chúng có cách thức thực hiện khác nhau cho các method của interface đó.
>3. Qua đó cùng 1 phương thức sẽ cho kết quả khác nhau khi được gọi bởi các đối tượng thuộc lớp khác nhau.
```PHP
<?php
abstract class Animal
{
	public function MakeSound()
	{
		return "An animal makes sound.";
	}
}

class Dog extends Animal
{
	public function MakeSound()
	{
		return "Bark";
	}
}

class Cat extends Animal
{
	public function MakeSound()
	{
		return "Meow";
	}
}

class Pig extends Animal
{
	public function MakeSound()
	{
		return "Oink";
	}
}
?>
```

#### Tính đóng gói (Encapsulation)
>Tính đóng gói được thể hiện thông qua việc giới hạn quyền truy cập, quyền thay đổi giá trị các property hay quyền gọi tới các method của đối tượng. Để có thể thay đổi hay truy cập tới các property ta chỉ có thể sử dụng các method mà cho phép ta gọi nó. Tính đóng gói giúp đảm bảo sự toàn vẹn dữ liệu của đối tượng.
>Trong PHP, tính đóng gói được thể hiện qua các keywords như public, private và protected:
>1. public: Cho phép truy cập và thay đổi giá trị của property và method ở mọi phạm vi
>2. protected: Chỉ cho phép truy cập và thay đổi giá trị của property và method ở phạm vi của đối tượng con (hoặc lớp con)
>3. private: Chỉ cho phép truy cập và thay đổi giá trị của thuộc tính và phương thức ở phạm vị của đối tượng (hoặc lớp)

#### Tính kế thừa (Inheritance)
>Tính kế thừa cho phép một lớp con có thể kế thừa toàn bộ property, method của lớp cha. Trong tính kế thừa, lớp con có thể sử dụng toàn bộ các property, method của lớp cha, mở rộng các chức năng của lớp cho cha bằng việc thêm vào một số property, method mới hoặc ghi đè lại các method của lớp cha để thay đổi hành vi của chúng.
>Trong PHP, tính kế thừa được thể hiện thông qua cú pháp extends như trong các ví dụ trên.

### Thế nào là một phương thức static ?
>Phương thức static là phương thức có thể truy cập mà không cần khởi tạo một đối tượng của class.
>Phương thức static gắn liền với class hơn là với object, đây là những phương thức chỉ có một, có thể xác định và không thay đổi địa chỉ trên vùng nhớ (tĩnh).
>Khi chương trình chạy, nó sẽ được sinh ra đầu tiên trước tất cả các truy nhập tới nó và tồn tại cho đến khi chương trình kết thúc.
>**Chú ý:** Trong hàm static không thể gọi hàm hoặc thuộc tính non-static. Nhưng hàm non-static có thể gọi hàm hoặc thuộc tính static do các thuộc tính và hàm static ở dạng toàn cục, được gọi mà không cần khởi tạo nên nếu bạn dùng từ khóa `$this` để gọi đến một hàm nào đó trong chính lớp đó thì sẽ bị báo sai.