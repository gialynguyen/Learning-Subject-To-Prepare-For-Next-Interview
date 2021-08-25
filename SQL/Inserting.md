Giả sử như ta có một table (PostgreSQL) `customer`:

| Column |                               |
| ------ | ----------------------------- |
| id     | integer default auto-increase |
| name   | varchar(50)                   |
| email  | varchar(50)                   |
| age    | smallint                      |
| phone  | varchar(20)                   |
| is_admin | bool default false			|

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

3. Copying a Table Definition:

Khi bạn muốn tạo mới một table có cấu trúc tương tự với một bảng khác sẵn có, ví dụ như ta sẽ khởi tạo một bảng `admin` có cùng cấu trúc như table `customer` bên trên.

Đối với DB2, thì chúng ta đã được cung cấp sẵn một mệnh đề `like` để thực hiện chức năng nên:

```sql
	create table admin like customer
```

Còn đối với MySQL, PostgreSQL, Oracle, ta sẽ dùng một thủ thuật: Chúng ta sẽ khởi tạo một bảng mới bằng một câu lệnh query với một điều kiện đảm bảo không có record nào đáp ứng
(Điều kiện luôn luôn sai). Ta sẽ thực hiện như sau:

```sql
	create table admin
	as 
	select *
		from customer
	where 1 = 0
```

Khi ta thực hiện Create Table As Select (CTAS), thì tất cả các row từ câu lệnh query sẽ được sử dụng để populate sang bảng mới được tạo. Nhưng khi ta thực hiện query với một điều kiện 
luôn luôn sai, ta sẽ đảm bảo được không có record nào sẽ được populate sang bảng mới, thay vào đó là một table rỗng có cấu trúc column tương tự như table ban đầu.

Đối với SQL Server, ta cũng sẽ thực hiện với cùng một cách như trên, nhưng cấu tạo câu lệnh sẽ có đôi chút khác, cụ thể như sau: 

```sql
	select *
		into admin
		from customer
	where 1 = 0
```

4. Copying Rows from One Table into Another

Khi ta muốn copy row từ table này sang table khác, trước hết ta phải đảm bảo 2 table có các column (được copy) phải có cùng cấu trúc (tên, kiểu dữ liệu). Khi này ta có thể dễ dàng
copy row thông qua các câu lệnh query.

Ví dụ như ta có table `admin` như sau:

| Column |                               |
| ------ | ----------------------------- |
| id     | integer default auto-increase |
| name   | varchar(50)                   |
| email  | varchar(50)                   |
| phone  | varchar(20)                   |

Ta sẽ copy các row có `is_admin` bằng `true` từ table `customer` qua bên table `admin` bằng câu lệnh sau:

```sql
	insert into admin (name, email, phone)
	select name, email, phone
		from customer
	where is_admin = true
```

Còn nếu như ta muốn copy tất cả copy thì ta chỉ việc loại bỏ mệnh đề `where` đi mà thôi. Còn nếu như ta bỏ qua việc liệt kê cụ thể các column, thì lúc này ta phải đảm bảo được
tất cả số lương, cấu trúc và thứ tự của column ở hai table phải tương đồng với nhau nếu không muốn lỗi xảy ra.

5. Inserting into Multiple Tables at Once.

Vấn đề tương tự như trên, nhưng thay vì lấy tất cả các row sau khi query để insert vào một table nhất định thì ta sẽ insert chúng vào nhiều table cùng
một lúc.

Thông thường ở phần lớn các DBMS, tác vụ này thường không được hỗ trợ thực hiện một cách dễ dàng và chính quy. Tuy nhiên ta vẫn có thể thực hiện được
ở một số DBMS cụ thể như sau:

Đối với `Oracle` ta có thể sử dụng mệnh đề `INSERT ALL` để giải quyết vấn đề này:

```sql
	insert all
		when is_admin = true then
		into admin (name, email, phone) values (name, email, phone)
		else
		into user (name, email, phone) values (name, email, phone)
	select name, email, phone
	from customer
```

Đối với `DB2`, ta sẽ có 1 thủ thuật và phải cân nhắc thật kỹ khi apply vào trong thực tế: Đó là ta khi khởi tạo các table ta sẽ tạo các `constraint` đóng vai trò như các bộ filter để đảm bảo các record sẽ được insert vào đúng table. Lúc này ta sẽ insert vào môt `view` được định nghĩa từ `UNION ALL` của tất cả các table cần insert vào. Tại sao lại là `UNION ALL`? Vì sẽ có các table có cùng `constraint`, nên khi thực hiện insert, `DB2` sẽ không biết phải insert vào table nào, điều này sẽ tạo ra lỗi. Cụ thể ta sẽ thực hiện như sau

```sql
	create table admin (
		name varchar(50),
		email varchar(50),
		phone varchar(20),
		is_admin bool default true check (is_admin = true)
	)

	create table user (
		name varchar(50),
		email varchar(50),
		phone varchar(20),
		is_admin bool default false check (is_admin = false)
	)

	insert into (
		select * from admin union all
		select * from user union all
	) select * from customer
```

Còn đối với các DBMS khác như MySQL, PostgreSQL, SQL Server thì ở thời điểm hiện tại, ta sẽ không có cách nào đủ đơn giản và hiệu quả để giải quyết vấn đề trên.
