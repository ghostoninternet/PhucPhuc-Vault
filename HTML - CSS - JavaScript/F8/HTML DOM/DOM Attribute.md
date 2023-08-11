1. Lấy được element
2. Đứng từ element đó, chấm rồi gõ tên của attribute tương ứng với element đó
>`var headingElement = document.querySelector('h1')`
>`headingElement.title = "Heading"`
3. Cách thêm attribute vào element mà attribute đó theo mặc định là không tương thích với element đó.
>`headingElement.setAttribute('<tên attribute>', '<giá trị của attribute đó>')`
4. Cách lấy ra giá trị của attribute của element
>`headingElement.getAttribute('<tên của attribute muốn lấy giá trị>')`
>Hoặc sử dụng cách sau, tuy nhiên chỉ áp dụng với attribute đã tương thích sẵn với element đó
>`headingElement.title` 