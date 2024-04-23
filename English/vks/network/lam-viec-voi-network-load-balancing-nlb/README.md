# Làm việc với Network load balancing (NLB)

### NLB là gì? <a href="#workingwithnetworkloadbalancing-nlb-nlblagi" id="workingwithnetworkloadbalancing-nlb-nlblagi"></a>

* &#x20;**Network Load Balancer (NLB)** là một bộ cân bằng tải được cung cấp bởi VNGCloud giúp phân phối lưu lượng truy cập mạng đến nhiều máy chủ back-end (backend servers) trong một nhóm máy tính (instance group). NLB hoạt động ở layer 4 của mô hình OSI, giúp cân bằng tải dựa trên địa chỉ IP và cổng TCP/UDP. Để biết thêm thông tin chi tiết về NLB, vui lòng tham khảo tại [How it works (NLB)](https://docs-admin.vngcloud.vn/pages/viewpage.action?pageId=64553859)

#### Mô hình triển khai <a href="#workingwithnetworkloadbalancing-nlb-mohinhtrienkhai" id="workingwithnetworkloadbalancing-nlb-mohinhtrienkhai"></a>

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

* **vngcloud-controller-manager**: VNG Cloud Controller Manager là một bộ điều khiển chạy trên các cụm Kubernetes được triển khai trên VNG Cloud. Nó chịu trách nhiệm cho việc quản lý các tài nguyên VNG Cloud cho các cụm Kubernetes, bao gồm:
  * **Tạo và quản lý Network Load Balancer (NLB)** cho các Service Kubernetes có service type = Load Balancer.
