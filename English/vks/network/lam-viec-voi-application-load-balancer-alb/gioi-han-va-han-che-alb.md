# Giới hạn và hạn chế ALB

## Giới hạn <a href="#restrictionsandlimitationsalb-gioihan" id="restrictionsandlimitationsalb-gioihan"></a>

Một vài lưu ý về giới hạn của việc ingress an ALB vào một cluster:&#x20;

* Một cluster có thể có nhiều Ingress, mỗi Ingress có chứa thông tin tài nguyên cho một ALB duy nhất.
* Một ALB có thể được sử dụng chung cho nhiều Ingress. Từ đó một ALB có thể được sử dụng chung cho nhiều cluster nhưng phải đảm bảo các cluster này có chung **Subnet**.
* Một ALB có thể bao gồm nhiều listener, nhiều pool, nhiều policy. Các giới hạn về số lượng listener, số lượng pool, số lượng policy vui lòng tham khảo tại [Hạn mức tài nguyên](https://docs-admin.vngcloud.vn/pages/viewpage.action?pageId=59802094).

***

## Hạn chế <a href="#restrictionsandlimitationsalb-hanche" id="restrictionsandlimitationsalb-hanche"></a>

* Ingress controller manager cần phải cấu hình các annotation để chỉ định các thuộc tính của ALB, như protocol, port,... Các annotation này có thể khác nhau tùy theo nhà cung cấp dịch vụ đám mây, hiện tại với **vngcloud ingress controller** đang cung cấp danh sách annotation tham khảo tại  [cau-hinh-cho-mot-application-load-balancer.md](cau-hinh-cho-mot-application-load-balancer.md "mention"). Chúng tôi sẽ nâng cấp thêm phần này trong các phiên bản release kế tiếp.
* Ngoài mục Kubernetes Cluster, các mục khác như Load Balancer, Volumes cũng được liệt kê bên ngoài mục Kubernetes Cluster, cụ thể là các mục Load Balancer, Volumes trong vServer Portal. Nếu bạn đổi tên hoặc sửa đổi các tài nguyên này (Load Balancer, Volumes) trong vServer Portal, bạn có thể khiến chúng không sử dụng được cho Cluster hoặc khiến cho tài nguyên trên vServer Portal và tài nguyên trên Cluster không được đồng nhất. Để tránh điều này, hãy quản lý tài nguyên cluster của bạn với **kubectl**.
