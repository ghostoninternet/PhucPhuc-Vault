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
>```PHP
```
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
### Tính đa hình (Polymorphism)
