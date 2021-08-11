## 1. Inserting a new record (Most Basically)

**Problem**

Giả sử như ta có một table Customer (có các column như bên dưới) và ta muốn insert 1 record mới vào nó.

| Column |     |
| ----------- | ----------- |
| id | integer default auto-increase|
| name | varchar(50) |
| email | varchar(50) |
| phone | varchar(20) |

Giá trị các column của record mới lần lượt có `name` là `Tom`, `email` là `customer01@gmail.com`, phone là `+84986746145`.

**Solution**

Sử dụng 