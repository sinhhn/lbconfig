# Load balancing, affinity, persistence, sticky sessions: what you need to know
## Tóm tắt
Trong ứng dụng web, để đảm bảo hiệu năng và tính sẵn sàng của hệ thống chúng ta thường sử dụng một khái niệm đó là **load balancer**.

## Kiến trúc của load balancer
Chúng ta hãy xem hình dưới đây:

![Load Balancer](https://cdn.haproxy.com/blog/wp-content/uploads/2011/09/reverse_proxy.png)

Trên hình vẽ trên, chúng ta có thể thấy hiểu một cách nôm na rằng, load balancer là 1 thiết bị đảm nhận phân tải (request) từ user đến các server khác nhau. Tức là, khi user request đến 1 địa chỉ, trước hết nó sẽ đi qua load balancer. Tại loan balancer, request đó sẽ được điều hướng đến một server được config trước. Điều này làm giảm tải cho các server.

Tuy nhiên, vấn đề đặt ra ở đây là gì?

Trước hết, chúng ta hãy để ý rằng, ứng dụng web chạy trên giao thức HTTP mà trong HTTP thì section độc lập với TCP connection. Tức là một section từ user sẽ không biết được TCP connection mà nó cần. Vậy thì điều này gây ra hệ luỵ gì? Nếu chúng ta không sử dụng LB thì bản thân web server luôn đảm bảo được về section của tất cả user. Chính vì vậy, dù có bao nhiêu client đang kết nối đến thì nó cũng đảm bảo rằng đang đến đúng đích nó cần.

Khi chúng ta sử dụng nhiều server, vấn đề sẽ xảy ra. Bạn hãy tưởng tượng như sau. User A có section 1 đang được kết nối vào server 1. User đó ấn refresh và thật không may LB điều họ đến server 2. Ngay lập tức user A được xem như 1 user mới đối với server 1. Đây là vấn đề rất cơ bản của LB. Để giải quyết vấn đề này, chúng ta có 1 số cách như sau:

* Lưu section trong database
* IP source affinity to server
* Application layer persistence

