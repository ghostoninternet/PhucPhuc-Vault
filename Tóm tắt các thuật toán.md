# Thuật toán 1: Tìm bao đóng tập thuộc tính trên một tập phụ thuộc hàm
(min --> max)
Input: tập thuộc tính cho trước
Output: bao đóng tập thuộc tính đấy
# Thuật toán 2: Tìm khóa tối thiểu
Tìm tập thuộc tính chứa số lượng thuộc tính tối thiểu mà từ đó vẫn có thể xác định được các thuộc tính khác (max --> min)
Input: tập thuộc tính + tập phụ thuộc hàm
Output: khóa tối thiểu
# Thuật toán 3: Tìm tập phụ thuộc hàm không dư thừa
Tìm tập phụ thuộc hàm tối thiểu mà từ đó vẫn suy ra được phụ thuộc hàm đầu bài đã cho, vẫn suy ra được bao đóng của tập phụ thuộc hàm
Input: tập phụ thuộc hàm có dư thừa
Output: tập phụ thuộc hàm tối thiểu
# Thuật toán 4: Tìm phủ tối thiểu
Input: tập phụ thuộc hàm
Output: phủ tối thiểu
Bước 1: Đưa vế phải về một thuộc tính
Bước 2: Loại bỏ dư thừa thuộc tính của vế trái (thuật toán 1)
Bước 3: Loại bỏ tập phụ thuộc hàm dư thừa (thuật toán 3)
# Thuật toán 5: Thuật toán xác định phép tách bảo toàn thông tin
Input: sơ đồ quan hệ, tập phụ thuộc hàm, phép tách
Output: phép tách trên input có mất mát thông tin ?
Cho k bảng và n thuộc tính
Bước 1: Lập ma trận k hàng và n thuộc tính
Bước 2: Xác định các ô mà tại đó thuộc tính Aj có tồn tại trong Ri và đánh dấu ô đó là aj, còn lại đánh là bij
Bước 3: Dựa vào các phụ thuộc hàm để chỉnh sửa lại các ô
Bước 4: Nếu có 1 hàng toàn a --> bảo toàn
