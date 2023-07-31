## 1. Thế nào là Repository, Branch ?
**Repository**: là nơi chứa tất cả các folder, các file liên quan đến project, lịch sử commit, nội dung commit của cá nhân mỗi người. Dữ liệu của Repository được lưu ở trong folder ẩn tên là `.git`. Để có thể theo dõi sự thay đổi và quản lý các commit của một project, đầu tiên cần vào folder chứa project đó, sau đấy chạy lệnh `git init`, lệnh này sau khi chạy sẽ sinh ra folder `.git`, và sau đấy cá nhân có thể quản lý và theo dõi các thay đổi và commit trên project này. Có hai loại repository: 
- Local repository: đây là repository nằm trên máy tính cá nhân của mỗi người.
- Remote repository: đây là repository nằm trên một server chuyên dụng, ví dụ như GitHub. Repository này có thể là của cá nhân hoặc của rất nhiều người cùng hợp tác.

**Branch**: là một nhánh của repository. Khi khởi tạo ra một repository, mặc định ban đầu người dùng sẽ nằm trên branch `main` (hoặc `master`). Một repository có thể có rất nhiều branch, mỗi branch sẽ có những mục đích khác nhau. Thông thường, branch `main` (hoặc `master`) sẽ là nơi lữu trữ phiên bản ổn định nhất của một project. Giả sử nếu cá nhân có muốn thay đổi hay update, fix bug trong project của mình, thì nên tách từ phiên bản ổn định nhất trên branch `main` (hoặc `master`) ra một branch khác. Mọi sự sửa đổi, commit trên branch mới sẽ không làm ảnh hưởng đến branch khác. Các branch có thể được tập hợp lại với nhau thông qua lệnh `git merge`

Có 2 loại branch:
- Branch local: là branch lưu ở local. Nó có thể được liên kết với một branch ở remote hoặc không. Để hiển thị branch có trên local, ta dùng lệnh `git branch`
- Branch remote: là branch lưu ở remote. Branch này có thể fetch về local thông qua lệnh `git fetch`, nhưng không tạo thêm branch ở local. Để hiển thị branch remote có trên local dùng lệnh `git branch -r`

## 2. Cách xóa một branch
**Xóa một branch phía local
- Cách 1: `git branch --delete <branch_name>` hoặc `git branch -d <branch_name>`
- Cách 2: `git branch --delete --force <branch_name>` hoặc `git branch -D <branch_name>`
**Xóa một branch phía remote**:
`git push <remote_name> --delete <branch_name>`
Chú ý: Phải check out ra branch cùng tên với branch trên remote thì mới xóa được. Ví dụ như khi ta đang ở branch `dev` thì ta không thể xóa branch `main` trên remote.

## 3. Làm thế nào để push một branch ở local lên remote dưới một cái tên khác
