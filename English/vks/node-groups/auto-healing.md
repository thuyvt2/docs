# Auto Healing

Trên hệ thống VKS, tính năng Auto Healing được apply cho mỗi Node Group và luôn luôn ở trạng thái bật. Việc bật tính năng tự phục hồi (self-healing) trong Kubernetes mang lại nhiều lợi ích quan trọng, giúp đảm bảo tính sẵn sàng và độ tin cậy cao cho ứng dụng của bạn. Cụ thể:&#x20;

**1. Giảm thiểu thời gian gián đoạn:**

* Khi có node hoặc pod bị lỗi, Kubernetes sẽ tự động khởi động lại hoặc tạo mới pod để thay thế, đảm bảo ứng dụng của bạn luôn hoạt động mà không bị gián đoạn.
* Tính năng này đặc biệt hữu ích cho các ứng dụng quan trọng cần hoạt động liên tục 24/7, ví dụ như website thương mại điện tử hoặc hệ thống thanh toán trực tuyến.

**2. Tăng cường khả năng chịu lỗi:**

* Kubernetes có thể tự động phát hiện và xử lý các lỗi xảy ra trong cluster, chẳng hạn như lỗi phần cứng, lỗi mạng hoặc lỗi phần mềm.
* Nhờ vậy, bạn không cần phải can thiệp thủ công để khắc phục sự cố, tiết kiệm thời gian và công sức.

**3. Cải thiện hiệu quả hoạt động:**

* Tự phục hồi giúp tự động hóa việc quản lý cluster, giảm thiểu gánh nặng cho quản trị viên hệ thống.
* Nhờ vậy, bạn có thể tập trung vào các công việc quan trọng khác, chẳng hạn như phát triển ứng dụng hoặc nâng cao trải nghiệm người dùng.

**4. Đảm bảo tính sẵn sàng cao:**

* Kubernetes với tính năng tự phục hồi có thể đảm bảo ứng dụng của bạn luôn sẵn sàng phục vụ người dùng, ngay cả khi có sự cố xảy ra.
* Điều này giúp nâng cao uy tín và thương hiệu của doanh nghiệp bạn.

**5. Dễ dàng sử dụng:**

* Việc bật tính năng tự phục hồi trong Kubernetes rất đơn giản và dễ dàng. Bạn có thể thực hiện thao tác này thông qua giao diện dòng lệnh (CLI) hoặc giao diện người dùng đồ họa (GUI).
