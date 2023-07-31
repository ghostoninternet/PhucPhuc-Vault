## 1. Thế nào là Repository, Branch ?
**Repository**: là nơi chứa tất cả các folder, các file liên quan đến project, lịch sử commit, nội dung commit của cá nhân mỗi người. Dữ liệu của Repository được lưu ở trong folder ẩn tên là `.git`. Để có thể theo dõi sự thay đổi và quản lý các commit của một project, đầu tiên cần vào folder chứa project đó, sau đấy chạy lệnh `git init`, lệnh này sau khi chạy sẽ sinh ra folder `.git`, và sau đấy cá nhân có thể quản lý và theo dõi các thay đổi và commit trên project này. Có hai loại repository: 
- Local repository: đây là repository nằm trên máy tính cá nhân của mỗi người.
- Remote repository: đây là repository nằm trên một server chuyên dụng, ví dụ như GitHub. Repository này có thể là của cá nhân hoặc của rất nhiều người cùng hợp tác.

**Branch**: là một nhánh của repository. Khi khởi tạo ra một repository, mặc định ban đầu người dùng sẽ nằm trên branch `main` (hoặc `master`). Một repository có thể có rất nhiều branch, mỗi branch sẽ có những mục đích khác nhau. Thông thường, branch `main` (hoặc `master`) sẽ là nơi lữu trữ phiên bản ổn định nhất của một project. Giả sử nếu cá nhân có muốn thay đổi hay update, fix bug trong project của mình, thì nên tách từ phiên bản ổn định nhất trên branch `main` (hoặc `master`) ra một branch khác. Mọi sự sửa đổi, commit trên branch mới sẽ không làm ảnh hưởng đến branch khác. Các branch có thể được tập hợp lại với nhau thông qua lệnh `git merge`

Có 2 loại branch:
- Branch local: là branch lưu ở local. Nó có thể được liên kết với một branch ở remote hoặc không. Để hiển thị branch có trên local, ta dùng lệnh `git branch`
- Branch remote: là branch lưu ở remote. Branch này có thể fetch về local thông qua lệnh `git fetch`, nhưng không tạo thêm branch ở local. Để hiển thị branch remote có trên local dùng lệnh `git branch -r`

## 2. Cách xóa một branch
**Xóa một branch phía local**
- Cách 1: `git branch --delete <branch_name>` hoặc `git branch -d <branch_name>`
Xóa branch nếu như branch không có thay đổi nào chưa merge
- Cách 2: `git branch --delete --force <branch_name>` hoặc `git branch -D <branch_name>`
Xóa branch ngay cả khi branch có thay đổi chưa merge

**Xóa một branch phía remote**:
`git push <remote_name> --delete <branch_name>`
Chú ý: Phải check out ra branch cùng tên với branch trên remote thì mới xóa được. Ví dụ như khi ta đang ở branch `dev` thì ta không thể xóa branch `main` trên remote.

## 3. Làm thế nào để push một branch ở local lên remote dưới một cái tên khác
`git push <remote_name> <local_branch>:<remote_branch>`

## 4. Thế nào là git rebase. Phân biệt rebase với merge

## 5. Thế nào là git fetch. Phân biệt fetch với pull
`git fetch`: copy tất cả các sự thay đổi trên remote repository lên remote branch để ta có thể nhìn được người khác đã thay đổi những gì mà không merge ngay lập tức
`git pull`: thu hết toàn bộ các thay đổi trên remote repository và merge ngay lập tức.
## 6. Thế nào là git stash
Git stash được sử dụng khi bạn muốn lưu toàn bộ các thay đổi trên branch hiện tại nhưng chưa commit. Câu lệnh này rất hữu dụng khi bạn đang làm dở công việc ở branch hiện tại nhưng cần đổi sang để làm công việc ở branch khác.
Để lưu toàn bộ nội dung công việc đang dở, sử dụng lệnh `git stash save` hoặc `git stash`
Một số lệnh với stash:
- Xem danh sách stash: `git stash list`
- Apply stash gần nhất và xóa stash đó: `git stash pop`
- Apply stash: `git stash apply stash@{<index>}`
- Xem nội dung stash: `git stash show stash@{<index>}`
- Xóa stash: `git stash drop stash@{<index>}`
- Xóa toàn bộ stash: `git stash clear`
## 7. Làm thế nào xoá bỏ trạng thái của một vài commit gần đây
Có 2 cách để thực hiện công việc này:
- Cách 1: `git revert <commit_id>`
		Lệnh này tạo commit đảo ngược commit có commit_id đã chọn, commit chỉ định bị xóa bỏ, các commit mới hơn vẫn được giữ nguyên.
- Cách 2: `git reset --hard <commit_id>`
		Lệnh này xóa toàn bộ các commit trước đó và đưa branch về trạng thái của commit có commit_id đã chọn.
## 8. Làm thế nào để gộp một vài commit thành 1 commit duy nhất
Để gộp nhiều commit thành 1 commit duy nhất, ta có thể sử dụng:
- Cách 1: `git rebase -i <id_commit_end>`
- Cách 2: `git rebase -i HEAD~<index>`
`<id_commit_end>` là id của commit cuối trong nhóm cần gộp. `<index>` là số commit cần gộp. Ta có các lựa chọn pick | squash | fixup các commit trước khi save.
## 9. Phân biệt git reset, git reset --hard, git reset --soft
**git reset**: `git reset <commit_id>`
Di chuyển HEAD về vị trí commit reset, các thay đổi của file vẫn được giữ nguyên nhưng bị loại khỏi stage
**git reset --hard**: `git reset --hard <commit_id>`
Di chuyển HEAD về vị trí commit reset và loại bỏ các thay đổi của file tính từ sau vị trí commit reset
**git reset --soft**: `git reset --soft <commit_id>`
Lệnh này chỉ di chuyển HEAD về vị trí commit reset, các thay đổi của file và trạng thái của stage vẫn được giữ nguyên
## 10. Thế nào là cherry-pick, khi nào thì dùng cherry-pick
Khi chúng ta muốn lấy **1 hay n commits từ 1 branch bỏ vào 1 branch khác** hay **commit 1 lần lên 2 branch** thì nên sử dụng **cherry-pick**. **Git cherry-pick** là một cách để checkout 1 commit bất kỳ tại 1 branch được chỉ định về branch hiện tại. Hay chính là cherry-pick sẽ bốc commit của 1 branch nào đó và áp dụng vào branch hiện tại. Chúng ta có các trường hợp sau:
**Lấy 1 commit từ 1 branch bỏ vào branch master**
```Git
git checkout master

git cherry-pick feature-A~1

# Hoặc chúng ta có thể chỉ định hash commit
git cherry-pick C2
```
--> Đưa thay đổi của commit C2 thuộc branch feature-A vào branch master

**Lấy n commits từ 1 branch bỏ vào master**
```Git
# Nếu muốn thêm 1 vài commit, không liên tục
git cherry-pick commit_id1 commit_id3

# Nếu muốn thêm 1 loạt commit lần lượt cạnh nhau
git cherry-pick commit_id1...commit_id5

# Với code trên, thì  commit_id1 sẽ ko được thêm vào
# Để đưa commit được tính vào trong branch muốn thêm thì 
git cherry-pick commit_id1^..commit_id5
```

**1 lần commit cho cả 2 branches**
```Git
# Đang ở branch-X, thực hiện commit để tạo ra commit A
git add -A
git commit -m " finish commit A"

# Checkout sang branch Y và dùng cherry-pick nào
git checkout branch-Y
git cherry-pick branch-X
```
Với lệnh này, cherry-pick sẽ lấy commit cuối cùng ở branch-X và merge vào branch-Y.
## 11. Git Flow
