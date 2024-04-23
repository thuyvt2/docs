# Concept

Bên dưới là các concepts hiện tại VKS đang cung cấp cho bạn:&#x20;

## **1. Public Cluster and Public Node Group**

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Khi bạn khởi tạo một **Public Cluster với Public Node Group**, hệ thống VKS sẽ:&#x20;

* Tạo VM có Floating IP ( tức có IP Public). Lúc này các VM (Node) này có thể join trực tiếp vào cụm K8S thông qua Public IP này. Bằng cách sử dụng Public Cluster và Public Node Group, bạn có thể dễ dàng tạo các cụm Kubernetes có thể truy cập được từ internet. Điều này có thể hữu ích cho các ứng dụng cần được truy cập từ bên ngoài, chẳng hạn như ứng dụng web hoặc API. Nói cách khác, việc sử dụng Public Cluster và Public Node Group giúp bạn mở rộng khả năng kết nối của cụm Kubernetes của mình, cho phép các ứng dụng chạy trên cụm có thể được truy cập một cách dễ dàng từ bất cứ đâu trên internet, đồng thời vẫn đảm bảo tính linh hoạt và quản lý tập trung. Điều này làm tăng khả năng tiếp cận và phân phối ứng dụng của bạn đến người dùng cuối.&#x20;

## **2. Public Cluster and Private Node Group**

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

Khi bạn khởi tạo một **Public Cluster với Private Node Group**, hệ thống VKS sẽ:&#x20;

* Tạo VM không có Floating IP ( tức không có IP Public). Lúc này các VM (Node) này không thể join trực tiếp vào cụm K8S. Để các VM này có thể join vào cụm K8S, bạn cần phải sử dụng một NAT Gateway (NATGW). NATGW hoạt động như một trạm chuyển tiếp, cho phép các VM kết nối với cụm K8S mà không cần IP Public. Với VNG Cloud, chúng tôi khuyến cáo bạn sử dụng Pfsense như một NATGW cho Cluster của bạn. Pfsense sẽ giúp bạn quản lý lưu lượng mạng đến và đi (inbound và outbound traffic) một cách hiệu quả, đảm bảo an ninh mạng và quản lý truy cập. Nói cách khác, việc sử dụng Private Node Group trong một Public Cluster yêu cầu một cách tiếp cận khác để kết nối các VM với cụm K8S. Bạn không thể truy cập trực tiếp từ internet như khi sử dụng Public Node Group, nhưng vẫn có thể quản lý kết nối thông qua NATGW cụ thể thông qua Pfsense để đảm bảo an toàn và kiểm soát lưu lượng mạng của cụm Kubernetes của bạn.

## **3. Private Cluster and Private Node Group**

Coming soon.

