Các cách lấy ra được Element:
1. ID: 
2. class
3. tag
4. CSS Selector
5. HTML collection  ([[HTML Collection and NodeList]])

`document.getElementById(<tag's ID>)`
--> Trả về một đối tượng duy nhất
`document.getElementsByClassName(<class name>)`
`document.getElementsByTagName(<tag's name>)`
--> Trả về nhiều đối tượng
`document.querySelector(<Cấu trúc viết như CSS Selector>)`
--> Trả về phần tử đầu tiên tương ứng, cho dù có nhiều phần tử phía sau có cùng CSS Selector.
`document.querySelectorAll(<Cấu trúc viết như CSS Selector>)`
--> Trả về một **NodeList** các phần tử 