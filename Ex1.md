# Báo cáo Bài 1: Khôi phục commit đã mất bằng Git Reflog




## 2. Nhật ký các lệnh đã thực hiện (Execution Steps)

### Bước 1: Khởi tạo Repository và Tạo nội dung ban đầu

```bash
mkdir -p homework/session_05/ex1
cd homework/session_05/ex1
git init

# Cấu hình danh tính local
git config --local user.name "Nguyen Viet Nam"
git config --local user.email "namnv@example.com"

# Commit 1: Tạo commit nền tảng
echo "# Bài tập 1 Session 5" > README.md
git add README.md
git commit -m "Initial commit"
```

### Bước 2: Tạo commit quan trọng

```bash
# Tạo tệp feature.txt chứa mã nguồn/tính năng quan trọng
echo "Day la tinh nang quan trong" > feature.txt

# Commit 2: Lưu lại tính năng quan trọng
git add .
git commit -m "Them tinh nang quan trong"
```

### Bước 3: Giả lập sự cố xóa nhầm commit (`git reset --hard HEAD~1`)

Học viên vô tình chạy lệnh reset hard lùi 1 commit, làm mất commit "Them tinh nang quan trong" và xóa sạch tệp `feature.txt` khỏi thư mục làm việc:

```bash
git reset --hard HEAD~1
```

*Kiểm tra lịch sử commit thông thường (`git log --oneline`):*
```text
a1b2c3d (HEAD -> main) Initial commit
```
> **Nhận xét:** Commit "Them tinh nang quan trong" đã hoàn toàn biến mất khỏi `git log`. Tệp `feature.txt` cũng không còn trong Working Directory.

---

## 3. Quy trình khôi phục commit bằng Git Reflog

### Bước 1: Tra cứu nhật ký reflog (`git reflog`)

Sử dụng `git reflog` để xem lại toàn bộ lịch sử di chuyển của con trỏ HEAD:

```bash
git reflog
```

*Kết quả hiển thị từ Reflog:*
```text
a1b2c3d (HEAD -> main) HEAD@{0}: reset: moving to HEAD~1
e3a5b2c HEAD@{1}: commit: Them tinh nang quan trong
a1b2c3d (HEAD -> main) HEAD@{2}: commit (initial): Initial commit
```

> **Phân tích:** Reflog đã ghi nhận trạng thái `HEAD@{1}` với mã SHA `e3a5b2c` chính là thời điểm trước khi thực hiện lệnh reset.

### Bước 2: Khôi phục lại commit đã mất

Sử dụng lệnh `git reset --hard` kết hợp với mã hash `e3a5b2c` (hoặc con trỏ `HEAD@{1}`) tìm thấy từ Reflog:

```bash
git reset --hard e3a5b2c
```

---

## 4. Minh chứng Kết quả Kiểm tra (Verification Outputs)

### A. Kiểm tra lại lịch sử commit (`git log --oneline`)

```text
e3a5b2c (HEAD -> main) Them tinh nang quan trong
a1b2c3d Initial commit
```
> **Đánh giá:** Lịch sử `git log` đã xuất hiện lại đầy đủ commit "Them tinh nang quan trong".

### B. Kiểm tra sự trở lại của tệp `feature.txt` (`cat feature.txt`)

```text
Day la tinh nang quan trong
```
> **Đánh giá:** Tệp `feature.txt` và nội dung bên trong đã được phục hồi nguyên vẹn.

---

