### 2. Thêm 1 dòng dữ liệu trong bất kỳ table

```
INSERT INTO mysql_exercise.user
VALUE ( 15, 'Đặng Văn Nhớ', 'nho@example.com', 3, 1, now());
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

### 10. Lấy Category có tồn tại blog hoặc news đã active (không được lặp lại category)

```
SELECT DISTINCT * FROM category
WHERE id IN (SELECT DISTINCT category_id FROM blog WHERE is_active = 1
union
SELECT DISTINCT category_id FROM news WHERE is_active = 1);
```

### 11. Lấy tổng lượt view của từng category thông qua blog và news

```
SELECT category_id, SUM(sumview) AS total_views
FROM (
    SELECT category_id, SUM(`view`) AS sumview FROM blog GROUP BY category_id
    UNION
    SELECT category_id, SUM(`view`) AS sumview FROM news GROUP BY category_id
) AS Sum_view
GROUP BY category_id;
```

### 12. Lấy blog được tạo bởi user mà user này không có bất kỳ comment ở blog

```
SELECT * FROM blog WHERE user_id NOT IN
	(SELECT distinct user_id FROM `comment` WHERE target_table = 'blog');
```

### 13. Lấy 5 blog mới nhất và số lượng comment cho từng blog

```
SELECT b.*, COUNT(c.id) AS comment_count
FROM blog b
JOIN comment c ON b.id = c.target_id AND c.target_table = 'blog'
GROUP BY b.id
ORDER BY b.created_at DESC LIMIT 5;
```

### 14. Lấy 3 User comment đầu tiên trong 5 blogs mới nhất

```
SELECT DISTINCT `user`.* FROM `user` WHERE id IN
	(SELECT distinct `comment`.user_id FROM `comment`
	, (SELECT blog.* FROM blog ORDER BY created_at DESC LIMIT 5) as latest_blog
    where `comment`.user_id = latest_blog.user_id and target_table= 'blog') LIMIT 3;
```

### 15. Update rank user = 2 khi tổng số lượng comment của user > 20

```
UPDATE user
SET `rank` = 2
WHERE id in (
SELECT c.user_id
FROM  comment c
GROUP BY c.user_id
HAVING count(c.comment) > 20) ;
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

### 19. Lấy 5 blog và 5 news của 1 category bất kỳ

```
SET @rd = (select id from category order by rand() limit 1);
(SELECT id, category_id, title FROM blog where category_id = @rd  LIMIT 5)
	UNION
(SELECT id, category_id, title FROM news where category_id = @rd LIMIT 5);

select id from category order by rand() limit 1;
```

### 20. Lấy blog và news có lượt view nhiều nhất

```
select id , category_id from (
SELECT  * FROM blog
ORDER BY view Desc
LIMIT 1
) as b

UNION
select id , category_id from(
SELECT  * FROM news
ORDER BY view Desc
LIMIT 1
) as n;
```

### 21. Lấy blog được tạo trong 3 ngày gần nhất

```
SELECT * FROM mysql_exercise.blog
WHERE created_at >= CURDATE() - INTERVAL 3 DAY
ORDER BY created_at DESC;
```

### 22. Lấy danh sách user đã comment trong 2 blog mới nhất

```
SELECT u.*
FROM mysql_exercise.user u
JOIN (
    SELECT *
    FROM mysql_exercise.blog
    ORDER BY created_at DESC
    LIMIT 2
) d ON u.id = d.user_id ;
```

### 22. Lấy danh sách user đã comment trong 2 blog mới nhất

```
SELECT DISTINCT `user`.* FROM `user` WHERE id IN
	(SELECT distinct `comment`.user_id FROM `comment`
	, (SELECT blog.* FROM blog ORDER BY created_at DESC LIMIT 2) as latest_blog
    where `comment`.user_id = latest_blog.user_id and target_table= 'blog');
```

### 25. Lấy 5 blog và 5 news mới nhất đã active

```
SELECT category_id , title , content , is_active FROM
(SELECT * FROM mysql_exercise.blog
WHERE is_active = 1
ORDER BY created_at DESC
LIMIt 5 ) AS B

UNION

SELECT category_id , title , content , is_active FROM
(SELECT * FROM mysql_exercise.news
WHERE is_active = 1
ORDER BY created_at DESC
LIMIt 5 ) AS N;
```

### 27. Blog của user đang được user có id = 1 follow

```
SELECT distinct b.*
FROM mysql_exercise.blog b , mysql_exercise.follow f
WHERE  b.user_id = f.from_user_id AND f.from_user_id = 1;
```

### 28. Lấy số lượng user đang follow user = 1

```
SELECT  count(from_user_id) AS 'SOLUONG'
FROM mysql_exercise.follow
WHERE to_user_id = 1;
```

### 29. Lấy số lượng user 1 đang follow

```
SELECT  count(to_user_id) AS 'SOLUONG'
FROM mysql_exercise.follow
WHERE from_user_id = 1 ;
```

### 31. SELECT CONCAT('PHP Team ', NOW()) AS result

```
SELECT CONCAT('PHP Team ', NOW()) AS INFO ;
```

### 32. Tìm có tên(user.full_name) "Khiêu" và các thông tin trên blog của user này như: (blog.title, blog.view), title category(category) của blog này. Hiển thị theo output như bên dưới:

```
SELECT u.fullname, b.title, b.view, c.title AS 'category_title'
FROM user u
JOIN blog b ON u.id = b.user_id
JOIN category c ON b.category_id = c.id
WHERE u.fullname LIKE '%Khiêu%';
```

### 33. Liệt kê email user các user có tên(user.full_name) có chứa ký tự "Khi" theo danh sách như output bên dưới.

```
SELECT email FROM user
WHERE fullname LIKE '%Khi%'
```

### Những câu chưa làm:

23 , 24 , 26 , 34
