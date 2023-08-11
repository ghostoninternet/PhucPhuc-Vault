1. **Attribute event**: Định nghĩa luôn event vào bên trong các thẻ tag HTML, thêm tiền tố **on** trước các tên event
>`<h1 onclick="console.log(Math.random())">DOM Event</h1>`

>Hiện tượng nổi bọt: Khi click vào element con của một element cha đang lắng nghe sự kiện, thì đầu tiên sự kiện đó sẽ được lắng nghe trên element con trước, sau đó nó nổi bọt ra ngoài, tức là được lắng nghe bởi các element cha bên ngoài.

2. **Assign event using the element node**: 
>`<Element node>.on<tên event> = function() {<Kịch bản sẽ chạy khi event được kích hoạt>}`
