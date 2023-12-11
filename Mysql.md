### 2. Thêm 1 dòng dữ liệu trong bất kỳ table

```
INSERT INTO mysql_exercise.user
VALUE ( 15, 'Đặng Văn Nhớ', 'nho@example.com', 3, 1, now())
```

### 3. Xoá và sửa1 dòng dữ liệu trong bất kỳ table nào

```
DELETE FROM mysql_exercise.user
WHERE id = 15 ;

UPDATE mysql_exercise.user
SET `rank` = 8
WHERE fullname = 'Hồ Bảo';
```

### 4. Select 10 blog mới nhất đã active

```
SELECT * FROM mysql_exercise.blog
WHERE is_active = 1
ORDER BY created_at DESC
LIMIT 10;
```

### 5. Lấy 5 blog từ blog thứ 10

```
SELECT * FROM mysql_exercise.blog
LIMIT 5 OFFSET 9;
```

### 6. Set is_active = 0 của user có id = 3

```
UPDATE mysql_exercise.blog
SET is_active = 0
WHERE id = 3;
```

### 7. Xoá tất cả comment của user = 2 trong blog = 5

```
DELETE  FROM mysql_exercise.comment
WHERE user_id = 2 AND target_table = 'blog' AND target_id = 5 ;
```

### 8. Lấy 3 blog bất kỳ (random)

```
SELECT * FROM mysql_exercise.blog
ORDER BY RAND()
LIMIT 3 ;
```

### 9. Lấy số lượng comment trung bình trên mỗi blog (Update: Lấy số lượng comment trung bình của tất cả blog)

```
SELECT AVG(count_cmt) FROM (SELECT COUNT(comment.id) count_cmt FROM blog
	LEFT JOIN comment ON (comment.target_id = blog.id && comment.target_table = 'blog')
    GROUP BY blog.id) avg_cmt;
```

### 15. Update rank user = 2 khi tổng số lượng comment của user > 20

```
UPDATE mysql_exercise.user AS u
JOIN (
    SELECT user_id, COUNT(comment.id) AS comment_count
    FROM comment
    GROUP BY user_id
    HAVING COUNT(comment.id) = 9
) AS c ON u.id = c.user_id
SET u.rank = 2;
```

### 16. Xoá comment mà nội dung comment có từ "fuck" hoặc "phức"

```
DELETE FROM mysql_exercise.comment
where comment LIKE '%fuck%' OR comment LIKE '%phức%';
```

### 17. Select 10 blog mới nhất được tạo bởi các user active

```
SELECT * FROM mysql_exercise.blog
WHERE is_active = 1
ORDER BY created_at DESC
LIMIT 10;
```

### 18. Lấy số lượng Blog active của user có id là 1,2,4

```
SELECT user.id , count(target_table) as 'So luong blog'
FROM mysql_exercise.comment , mysql_exercise.user
where comment.user_id = user.id
AND comment.target_table = 'blog'
AND user.is_active = 1
AND ( user.id = 1 OR user.id = 2 OR user.id = 4 )
GROUP BY user.id ;
```

### 21. Lấy blog được tạo trong 3 ngày gần nhất

```
SELECT * FROM mysql_exercise.blog
WHERE created_at >= CURDATE() - INTERVAL 3 DAY
ORDER BY created_at DESC;
```

### Những câu chưa làm

10.Lấy Category có tồn tại blog hoặc news đã active (không được lặp lại category)

11.Lấy tổng lượt view của từng category thông qua blog và news

12.Lấy blog được tạo bởi user mà user này không có bất kỳ comment ở blog

13.Lấy 5 blog mới nhất và số lượng comment cho từng blog

14.Lấy 3 User comment đầu tiên trong 5 blogs mới nhất

19.Lấy 5 blog và 5 news của 1 category bất kỳ

20.Lấy blog và news có lượt view nhiều nhất

22.Lấy danh sách user đã comment trong 2 blog mới nhất

23.Lấy 2 blog, 2 news mà user có id = 1 đã comment

24.Lấy 1 blog và 1 news có số lượng comment nhiều nhất

25.Lấy 5 blog và 5 news mới nhất đã active

26.Lấy nội dung comment trong blog và news của user id =1

27.Blog của user đang được user có id = 1 follow

28.Lấy số lượng user đang follow user = 1

29.Lấy số lượng user 1 đang follow

31.Hiển thị một chuổi "PHP Team " + ngày giờ hiện tại (Ex: PHP Team 2017-06-21 13:06:37)

32.Tìm có tên(user.full_name) "Khiêu" và các thông tin trên blog của user này như: (blog.title, blog.view), title category(category) của blog này. Hiển thị theo output như bên dưới:

33.Liệt kê email user các user có tên(user.full_name) có chứa ký tự "Khi" theo danh sách như output bên dưới.

34.Tính điểm cho user có email là minh82@example.com trong bảng comment. Cách tính điểm:
Trong bảng comment với taget_table = "blog" tính 1 điểm, taget_table = "news" tính 2 điểm.
