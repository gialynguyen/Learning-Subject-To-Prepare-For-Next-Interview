Giả sử như ta có một table (PostgreSQL) `customer`:

| Column |                               |
| ------ | ----------------------------- |
| id     | integer default auto-increase |
| name   | varchar(50)                   |
| email  | varchar(50)                   |
| age    | smallint                      |
| phone  | varchar(20)                   |

## 1. Inserting a new record (Most Basically)

**Problem**

Khi ta muốn insert 1 record (row) mới vào table Customer, ví dụ như giá trị các column của record mới lần lượt có `name` là `Tom`, age là `18`, `email` là `customer01@gmail.com`, phone là `+84986746145`.

**Solution**

Ta sử dụng lệnh INSERT đi kèm với mệnh đề VALUES:

```sql
	insert into customer (name, age, email, phone)
	values ('TOM', 18, 'customer01@gmail.com', '+84986746145')
```

Đặt biệt đối với một vài DBMS như DB2, SQL Server, PostgreSQL, MySQL,... chúng ta có thể
insert nhiều record cùng một lúc bằng cách sử dụng 1 list các bộ (set) VALUES:

```sql
	insert into customer (name, age, email, phone)
	values ('TOM', 18, 'customer01@gmail.com', '+84986746145')
		   ('JERRY', 17, 'customer02@gmail.com', '+84986747814')
```

Trên lý thuyết, ta có thể bỏ qua việc liệt kê các column khi thực hiện INSERT,
nhưng nếu làm như vậy ta phải insert value cho tất cả các column và phải nhớ chính xác
tất cả và thứ tự của các column của table được lưu trong DBMS. Cho nên, điều này là phi thực tế.

## 2. Default Value & Overriding a Default Value with NULL

Ta có thể đặt giá trị mặt định cho một column.
Ta có thể thực hiện nó ở bước khởi tạo table như sau: 

```sql
    create table [if not exist] roles (
		is_admin bool default false,
		is_user bool default true,
	)
```

hoặc khi thêm một column vào table: 

```sql
	alter table customer
	add column is_admin bool default false;
```

Lúc này ta có thể insert với các giá trị mặc định như sau:

```sql
	insert into roles values (default)
```

TUY NHIÊN một số DBMS sẽ không support DEFAULT keyword, ví dụ như Oracle8i.

Còn đối với MySQL, nó cho phép bạn tối giản câu lệnh hơn bằng cách ngầm hiểu khi bạn truyền một list VALUES rỗng, nó sẽ đặt các giá trị mặc định cho các column đã được config sẵn giá trị mặc định:

```sql
	insert into D values ()
```

Riêng PostgreSQL và SQL Server, chúng hỗ trợ một MỆNH ĐỀ (clause) `DEFAULT VALUES`:

```sql
	insert into roles default values
```

Ở một vài trường hợp đặt biệt, ta muốn đặc biệt insert 1 record mới bỏ đi giá trị của một số column cụ thể và tránh việc nhận giá trị default. Để làm được điều đó, ta có insert một giá trị NULL cho column đó như sau:

```sql
	insert into rules (is_user) values (null)
```