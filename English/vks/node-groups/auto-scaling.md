# Auto Scaling

Auto Scaling cho Cluster là một tính năng trong hệ thống quản lý cụm (cluster management) như Kubernetes, Docker Swarm, hoặc các dịch vụ điện toán đám mây, cho phép tự động điều chỉnh số lượng các node hoặc các tài nguyên trong cụm dựa trên tải công việc và yêu cầu của ứng dụng.

Tính năng Auto Scaling giúp đáp ứng một số vấn đề quan trọng trong việc quản lý cụm:

1. **Tối ưu hóa hiệu suất:** Auto Scaling cho phép cụm tự động mở rộng tài nguyên khi có nhu cầu. Khi tải công việc cao hơn, cụm sẽ tự động tạo thêm các node hoặc tài nguyên để đảm bảo các ứng dụng hoạt động với hiệu suất tốt nhất.
2. **Tiết kiệm chi phí:** Auto Scaling cho phép cụm tự động giảm quy mô tài nguyên khi không cần thiết. Nếu tải công việc giảm đi, cụm sẽ tự động thu hồi các tài nguyên không sử dụng để tiết kiệm chi phí.
3. **Đảm bảo tính sẵn sàng:** Auto Scaling giúp đảm bảo rằng cụm có sẵn để đáp ứng nhu cầu sử dụng và tránh tình trạng quá tải hoặc thiếu tài nguyên.
4. **Tự động phục hồi:** Auto Scaling giúp tự động phục hồi từ các sự cố hoặc lỗi bằng cách tạo ra các node mới để thay thế các node bị hỏng.
5. **Tích hợp với khái niệm về Infrastructure as Code (IaC):** Auto Scaling thường được tích hợp với các công cụ IaC như Terraform hoặc CloudFormation để định nghĩa và quản lý cơ sở hạ tầng của cụm dưới dạng mã, giúp tự động hóa việc triển khai và mở rộng tài nguyên.

Khi triển khai các ứng dụng trong môi trường cloud hoặc hệ thống phân tán, sử dụng tính năng Auto Scaling cho Cluster giúp tối ưu hóa sử dụng tài nguyên, cải thiện tính sẵn sàng và hiệu suất của ứng dụng, và giúp quản lý cụm trở nên dễ dàng và hiệu quả hơn.

***

### **Bật Auto Scaling** <a href="#autoscaling-batautoscaling" id="autoscaling-batautoscaling"></a>

Bạn có thể bật Auto Scaling khi:

* **Khởi tạo một Cluster**
* **Khởi tạo một Node Group**
* **Chỉnh sửa một Node Group**

Để bật Auto Scaling cho Kubernetes Cluster của bạn vui lòng bật lựa chọn **Enable Auto Scaling, khi bật lựa chọn này bạn cần nhập:**.

* **Minimum node**: số node tối thiểu mà Cluster cần có.&#x20;
* **Maximum node**: số node tối đa mà Cluster có thể scale tới.
* **Node Group upgrade stratetry**: chiến lược upgrade Node Group. Khi bạn thiết lập **Node Group Upgrade Strategy** thông qua phương thức **Surge upgrade** cho một Node Group trong VKS, hệ thống VKS sẽ cập nhật tuần tự để nâng cấp các node, theo thứ tự không xác định[.](https://cloud.google.com/kubernetes-engine/docs/concepts/node-pool-upgrade-strategies.)&#x20;
  * **Max surge:** giới hạn số lượng node được nâng cấp đồng thời (số lượng node mới (surge) có thể được tạo ra cùng một lúc). Mặc định **Max surge = 1** - chỉ nâng cấp một node tại một thời điểm.&#x20;
  * **Max unavailable**: giới hạn số lượng node không thể truy cập được trong quá trình nâng cấp (số lượng node hiện tại có thể bị gián đoạn cùng một lúc). Mặc định **Max unavailable = 0** - đảm bảo tất cả các node đều có thể truy cập được trong quá trình nâng cấp.

Ví dụ như hình bên dưới: tôi đã khởi tạo một Node Group với:

* **Number of nodes: 3 nodes**
* **Minimum node: 1 nodes**
* **Maximum node: 5 nodes**

<figure><img src="https://docs-admin.vngcloud.vn/download/attachments/73762025/image2024-4-17_11-45-7.png?version=1&#x26;modificationDate=1713329108000&#x26;api=v2" alt=""><figcaption></figcaption></figure>
