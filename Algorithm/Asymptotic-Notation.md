Sự hiệu quả của một thuật toán phụ thuộc vào sô lượng về thời gian, bộ nhớ và các resource khác mà nó sử dụng để thực thi. Trong Asymptotic Analysis, việc tính toán sự hiệu quả của một thuật toán không phải là một việc dễ dàng, vì thế nên các Asymptotic Notation được quy ước để giúp ích cho tính toán này.

Asymptotic Notations là các ký hiệu toán học được sử dụng để diễn tả thời gian thực thi của một thuật toán khi input của nó tiến về một giá trị cụ thể hoặc vô cực.

Thông thường thì sẽ có 3 Asymptotic Notations hay được nhắc đến:

- Big-O notation
- Omega notation
- Theta notation

### Big-O Notation (O)

Big-O (được đọc là Big - Oh) đại diện cho cận trên khi biểu thị thời gian thực thi của một thuật toán. Vì thế, nó cho đưa ra được `worst-case` của độ phức tạp thuật toán.

Khi ta nói `f(n)` là `O(g(n))`, điều này đồng nghĩa điều kiện là sẽ tồn tại một toán hạng `c` và `n0` thoải mãn:

```
	f(n) <= cg(n) với mọi n >= n0
```

Ta lấy ví dụ như sau: Ta sẽ chọn `g(n)` với

```
	g(n) = n^2
```

và `f(n)`:

```
	f(n) = 8*n^2 + 10*n
```

Chúng ta sẽ đi chứng mình `f(n) = O(g(n))`. Để làm được điều đó ta cần tìm ra được `c` và `n0` sao cho quan hệ sau được thoả mãn với mọi `n >= n0`:

```
	8*n^2 + 10*n <= c*n^2
```

Bằng cách chia 2 vế cho `n^2`, ta được biểu thức thư sau:

```
	8 + 10/n  <= c
```

Nếu ta chọn `n0 = 1`, thì giá trị của vế trái lúc này sẽ bằng 18. Tiếp theo ta chon `c = 19` và XONG. Chúng ta đã tìm được tồn tại 1 cặp `c = 19` và `n0 = 1` vì vậy ta có thể phát biểu

```
	8*n^2 + 10*n = 0(n^2)  // Xem lại phần trước, trong Asymptotic Analysis, các hằng số có thể được lượt bỏ khi n -> vô cực
```

Một vài ví dụ khác như:

```
	1/2*n^2 + 2n + 1= O(n^2)

	n = O(nlogn)

	logn = O(n)
```

### Omega Notation (Ω)

Ω (được đọc là Big - Omega) đại diện cho cận dưới khi biểu thị thời gian thực thi của một thuật toán. Vì thế, nó cho đưa ra được `best-case` của độ phức tạp thuật toán.

Khi ta nói `f(n)` là `Ω(g(n))`, điều này đồng nghĩa điều kiện là sẽ tồn tại một toán hạng `c` và `n0` thoải mãn:

```
	f(n) >= cg(n) với mọi n >= n0
```

Ví dụ như:

```
	10*n^2 + 14n+ 10 = Ω(n2)
```

Cùng một cách ở phần `Big-O` ta dễ dàng chứng minh được điều trên.

### Theta notation (Θ)

Θ (được đọc là Big - Theta) đại diện cho cận trên và cân dưới khi biểu thị thời gian thực thi của một thuật toán. Vì thế, nó cho đưa ra được `average-case` của độ phức tạp thuật toán.

Khi ta nói `f(n)` là `Θ(g(n))`, điều này đồng nghĩa điều kiện là sẽ tồn tại một toán hạng `c1`, `c2` và `n0` thoải mãn:

```
	0 < c1*g(n) <= f(n) <= c2*g(n) với mọi n >= n0
```

Ví dụ như:
```
 5*n^2 + 3n = Θ(n^2)
```

Để khẳng định điều đó ta sẽ cần tìm ra bộ 3 `c1`, `c2` và `n0` để luôn thoả mãn:

```
	c1*n^2 <= 5*n^2 + 3n <= c2*n^2
```

Tương tự, giả sử như ta chon `n0 = 1`, ta dễ dàng chọn được `c1 = 5` và `c2 = 9`