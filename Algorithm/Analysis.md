## Asymptotic Analysis

Khi cùng đưa vào 2 thuật toán đề giải quyết một task, bằng cách nào ta có thể cái nào là tốt hơn?
Theo một cách ngây thơ nhất, ta có thể triển khai và thực thi cả hai với các input khác nhau và xem thử xem thuật toán nào chiếm ít thời gian hơn.
Nhưng có nhiều vấn đề với hướng tiếp cận này, có thể kể đến như:

- Với một vài input cụ thể, thuật toán thứ nhất có thể nhanh hơn. Nhưng đối với những input khác, thuật toán thứ hai lại nhanh hơn đáng kể.
- Với một vài input, thuật toán thứ nhất có thể chạy nhanh hơn ở thiết bị này, nhưng khi thay đổi thiết bị thuật toán thứ hai lại thực thi nhanh hơn.

Lúc này `Asymptotic Analysis` xuất hiện như một ý tưởng để giải quyết những vấn đề bên trên trong việc phân tích thuật toán.
Trong `Asymptotic Analysis`, chúng ta đánh giá một cách phỏng đoán hiệu suất của một thuật toán dựa vào theo sự quan hệ với kích thước input (Chúng ta không đo lường thời gian chạy thực tế của thuật toán).
Chúng ta tính toán cách thức thời gian thực thi (hoặc không gian vùng nhớ) mà thuật toán chiếm lấy khi chúng ta tăng kích thước input (dữ liệu đầu vào).

Ví dụ, khi chúng ta phân tích thời gian thực thi của 2 thuật toán sắp xếp là `insertion sort` và `merge sort`.
`Insertion sort` sẽ cần một thời gian là `T(n)` được thể hiện thông qua phương trình `T(n) = c*n^2 + k`.
`Merge sort` sẽ cần một thời gian `T'(n)` và cũng được thể hiện qua phương trình `T'(n) = c'*n*log2(n) + k'`.
Thông thường khi áp dụng Asymptotic Analysis người ta thường hướng đến việc ước tính sự chậm chạp của một thuật toán khi nó phải nhận một input lớn,
cho nên những giá trị nhỏ của biến số `n` thường được bỏ qua.
Với cách tính toán này, một thuật toán được thể hiện với phương trình bậc một (`f(n) = c*n + k`) sẽ nhanh hơn thuật toán được thể hiện dưới dạng một phương trình bậc 2 (`f(n) = c*n^2 + k`).

Vì vậy, ta có thể khẳng đinh rằng, khi input lớn, thì `merge sort` sẽ chạy với thời gian là `n*log2(n)` (bỏ qua 2 hằng số c' và k')
và nó nhanh hơn `insertion sort` - có thời gian chạy là `n^2` (bỏ qua 2 hằng số là c và k).

*Note*: Ta có thể dễ dàng chứng minh như sau: 
```
     n log(n)
lim --------- = 0;
       n^2

	n log(n)            log(n)            1/n           1
lim ------------- = lim --------  =  lim ------- = lim ---- = 0
       n^2                 n               1            n
```

Điều này có nghĩa là `n^2` sẽ mức độ tăng lớn hơn `n*log2(n)`, hay nói cách khác, khi n hướng tới vô cùng, `merge sort` sẽ có thời gian chạy ít hơn `insertion sort`.

Asymptotic Analysis không phải lúc nào cũng là một giải pháp hoàn hảo.
Vì nó tập trung vào input có kich thước lớn, nhưng trong thực tế có thể phần mềm của chúng ta sẽ không được cung cấp một lượng input lớn đến như vậy,
cho nên sẽ có vài trường trường hợp, một thuật toán có `Asymptotic Analysis` không tốt nhưng lại là giải pháp tốt nhất cho phần mềm của bạn.

Ví dụ như ta có hai thuật toán được biểu thị bởi 2 phương trình lần lượt là `f(n) = 0.2*n` và `f'(n) = 1000*log(n)`,
theo như quy tắc tính của Asymptotic Analysis, thì rõ ràng thuật toán thứ 2 (f'(n)) sẽ nhanh hơn thuật toán thứ nhất.
Nhưng đó chỉ đúng khi input đạt đến một độ lớn nhất định, còn đối với khoảng input nhỏ hơn, thuật toán đầu tiên mới là lựa chọn tốt nhất cho phần mềm của chúng ta.

## Growth of Functions

Ở phần trên, ta đã nó đến sự tương quan giữa thời gian thực thi của một thuật toán với kích thước (độ lớn) của input (`size(n)`).
Chúng ta cũng đã xem xét, so sánh 2 thuận toán thông qua 2 phương trình tương ứng với từng thuật toán.
Phần này ta sẽ làm rõ hơn đồng thời thêm vài ví dụ minh hoạ về `Growth of Functions` thể hiện thời gian thực thi của thuật toán.

Giả sử chúng ta lần lượt có 3 phương trình đại diện cho 3 thuận toán lần lượt như sau:

```
f(n) = n;
f(n) = n^2;
f(n) = n ^3; 
```
Cách tiếp cận dễ nhất để xem xét thuật toán nào chạy nhanh hơn (ít thời gian hơn) là chúng ta sẽ thể hiện 3 phương trình trên lên toạ độ Oxy,để thấy được sự tương quan giữa n và t (thời gian).

![grow of function](./assets/grow_of_function.png)

Nhìn vào biểu đồ, ta dễ dàng nhận thấy rằng function `n^3` sẽ tăng vượt trội hơn `n^2` và `n`.
Vì vậy, thời gian thực thi của thuật toán thứ nhất (`n`) sẽ nhanh hơn thuật toán thứ 2 (`n^2`) và 3 (`n^3`).

Ngoài việc thể hiện một cách trực quan trên biểu đồ, ta có thể thông qua làm phép tính để biết được phương trình nào sẽ có mức tăng vượt trội hơn,
tương tự những gì ta đã làm ở cuối phần trước, chúng ta sẽ tính chia 2 phương trình cho nhau và tính giới hạn `lim(n -> ∞)` của chúng:

```
               f(n)
       lim =  ------
      n -> ∞   g(n)
```

Nếu giới hạn hướng là 0, f(n) sẽ tăng vượt trội hơn g(n). Ngược lại, nếu giới hạn hướng về ∞, g(n) sẽ tăng vượt trội hơn f(n).

Dưới đây là bảng sắp xếp theo thứ tự  thời gian thực thi tăng dần thường gặp trong phân tích thuyệt toán:

| Running Time| Examples    |
| ----------- | ----------- |
|Constant	|       1, 2, 100, 300, … |
|Logarithmic	|       logn, 5logn, …    |
|Linear	|              n, n+3, 2n+3, … |
|nlogn	       |       nlogn, 2nlogn+n, … |
|Polynomial	|       n^2, n^3 or higher order |
|Exponential	|       2^n, 3^n, 2^n + n^4, … |
|Factorial	|       n!, n! + n, … |

Hoặc trực quan hoá theo biểu đồ dưới đây:

![grow of function](./assets/growth_of_functions.png)

## Worst, Average and Best Cases
