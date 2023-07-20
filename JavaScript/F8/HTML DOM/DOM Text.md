# innerText, textContent

`var headingElement = document.querySelector('.heading');`
`console.log(headingElement.innerText)`
`console.log(headingElement.textContent)`
`headingElement.innerText = 'New heading'`
`headingElement.textContent = 'New heading'`

## Sự khác nhau giữa **innerText** và **textContent**
>**innerText**: là thuộc tính tồn tại trên **ELEMENT NODE**, cho ra text theo đúng như hiển thị trên trình duyệt. Tức là trên trình duyệt hiển thị như nào thì nội dung **innerText** sẽ như thế.
>
>**textContent**: à thuộc tính tồn tại trên **ELEMENT NODE** và cả **TEXT NODE**, cho ra nội dung text chứa bên trong element. Tức là nội dung mà **textContent** đưa ra bao gồm toàn bộ dấu cách, dấu xuống dòng tồn tại bên trong element đó. Thậm chí nếu bên trong có chứa một số thẻ như `<span>` hay `<style>` thì **textContent** vẫn ghi nhận nội dung chứa bên trong các thẻ đó.

