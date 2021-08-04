## Tổng Quan Về Docker

Docker là một platform dành cho developing, vận hành và triển khai application. Docker cho phép tách biệt các application ra khỏi infrastructure (cơ sở hạ tầng), vì thế phân phối phần một cách nhanh chống. Với docker ta có thể giảm đáng kể sự khác biệt môi trường vận hành dẫn đến trì hoãn giữa việc coding (development) và triển khai nó trên môi trường production.

Docker cung cấp khả năng đóng gói và thực thi một application trong một trường cô lập gọi là `container`. Nhờ vào đóng gọi một cách cô lập và bảo mật, ta có thể chứa đồng thời nhiều `container` trên cùng một `host` được cung cấp.

Container rất nhỏ gọn vừa đủ và bên trong chứa đầy đủ những thứ cần thiết cho application được vận hành, và nó không cần phải phụ thuộc vào những gì đang được cài đặt trên host hiện tại. Vì thế, ta có thể dễ dàng sharing container mà vẫn được đảm bảo rằng nó sẽ được thực thi đúng theo cùng một cách.

Docker cung cấp các tool đầy đủ để quản lý vòng đời của một container: 
- Develop application và các supporting components liên quan đến nó bằng cách sử dụng container.
- Container trở thành một unit (đơn vị) distributing (phân phối) và testing (kiểm thử) application.
- Khi bạn deploy application lên các môi trường production. Container sẽ hoạt động đúng theo cùng một cách thứ cho dù môi trường production là local data center, cloud service hay là sự kết hợp giữa cả hai.


## Kiến trúc Docker

Docker sử dụng kiến trúc client-server. `Docker client` giao tiếp với `Docker deamon` - một trình chạy nền thực hiện các tác vụ building, running và distributing các container. `Docker client` 