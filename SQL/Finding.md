Giả sử như ta có một table (PostgreSQL) `customer`:

| Column   |                               |
| -------- | ----------------------------- |
| id       | integer default auto-increase |
| name     | varchar(50)                   |
| email    | varchar(50)                   |
| age      | smallint                      |
| phone    | varchar(20)                   |
| is_admin | bool default false            |

## 1. Retrieving All Rows and Columns from a Table

**Problem**

Chúng ta có table `customer` và bây giờ ta muốn lấy tất cả data của nó.

**Solution**

Ta có thể sử dụng lệnh `SELECT` cùng với ký tự đặt biệt `*` như sau:

```sql
	select *
		from customer
```

Trong SQL, ký tự `*` có một ký nghĩa đặt biệt. Khi sử dụng nó sẽ trả về tất cả column của một table chỉ định. Bởi vì ta không sử dụng mệnh đề `WHERE` (ta sẽ tìm hiểu sau), cho nên tất cả các `row` sẽ được trả về. Đơn nhiên ta cũng có thể liệt kê cụ thể từng column:

```sql
	select id, name, email, age, phone, is_admin
		from customer
```

Trong trường hợp này, xét về mặt cú pháp, việc sử dụng SELECT * có lẽ dễ dàng và ngắn gọn hơn. Tuy nhiên, trong lập trình thực tế, việc chỉ ra cụ thể từng column sẽ là cách tiếp cận tốt hơn. Có thể về mặt performace sẽ là như nhau, nhưng với việc thực hiện tường minh bạn sẽ luôn biết được những column mà bạn đang muốn trả về từ câu lệnh query. Tương tự như vậy, câu lệnh cũng sẽ trở nên tường minh và dễ hiểu hơn đối với những đông nghiệp của bạn hơn. Cũng như sẽ đảm bảo câu lệnh sẽ trả vừa đủ những gì mà ta mong đợi. Và ít ra, nếu như có một vài column bị thiếu bên trong table thì sẽ có một `error` được `throw` ra, phần nào đó giúp ta `tracing` và pháp hiện `error` kịp thời.

## 2. Retrieving a Subset of Rows from a Table

**Problem**

Thay vì lấy hết tất cả data thì bây giờ ta chỉ muốn lấy các row thoả mãn một điều kiện cụ thể nào đó.

**Solution**

Cụ thể là ta sẽ tìm những customer có số tuổi là 18.
Ta sẽ sử dụng thêm mệnh đề `WHERE` để chỉ ra những `row` nào ta muốn lấy ra, như sau:

```sql
	select *
		from customer
	where age = 18
```

Mệnh đề `WHERE` cho phép ta chỉ giữ lại những `row` mà ta mong muốn. Nếu biểu thức ở mệnh đề WHERE đúng với bất cứ row nào thì row đó sẽ được trả về.

Hầu hết các DBMS đều hỗ trợ đầy đủ các toán tử cơ bản như `=, <, >, <=, >=. !, <>,...`. Thêm vào đó, ta cũng có thể muốn các row thoả mãn một lúc nhiều điều kiện, để làm được ta có thể sử dụng các mệnh đề khác như `AND`, `OR`,... ta sẽ trình bày nó ở phần sau.

## 3. Finding Rows That Satisty Multiple Conditions

**Problem**

Vấn đề đã được đề cập ở cuối phần trước, ta sẽ trả về các rows thoải mãn một lúc nhiều điều kiện.

**Solution**

Chúng ta sẽ sử dụng mệnh đề `WHERE` cùng với mệnh đề `OR` và `AND`. Ví dụ như ta sẽ trả về các admin có tuổi lơn hơn hoặc bằng 18 hoặc là user thường có tuổi lớn hơn hoặc bằng 24, như sau:

```sql
	select *
		from customer
	where ( age >= 18
	        and is_admin = true
		)
	or  ( 	age >= 24
			and is_admin = false
		)
```

Các cặp dâu ngoặc đơn sẽ nhóm các điều kiện bên trong chúng được đánh giá cùng với nhau, hãy cẩn thận sử dụng chúng nếu không muốn câu lệnh query sẽ không thực thi theo điều kiện ta mong muốn.
