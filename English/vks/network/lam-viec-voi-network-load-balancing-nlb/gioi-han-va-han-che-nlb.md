# Restrictions and limitations NLB

## Giới hạn <a href="#restrictionsandlimitationsnlb-gioihan" id="restrictionsandlimitationsnlb-gioihan"></a>

Một vài lưu ý về giới hạn của việc intergrate a NLB vào một cluster:&#x20;

* Một NLB có thể được sử dụng chung cho nhiều cluster nhưng phải đảm bảo các cluster này có chung **Subnet**.
* Một NLB có thể bao gồm nhiều listener, nhiều pool, nhiều policy. Các giới hạn về số lượng listener, số lượng pool, số lượng policy vui lòng tham khảo tại [Hạn mức tài nguyên](https://docs.vngcloud.vn/pages/viewpage.action?pageId=59802094).

***

## Hạn chế <a href="#restrictionsandlimitationsnlb-hanche" id="restrictionsandlimitationsnlb-hanche"></a>

* Ngoài mục Kubernetes Cluster, các mục khác như Load Balancer, Volumes cũng được liệt kê bên ngoài mục Kubernetes Cluster, cụ thể là các mục Load Balancer, Volumes trong vServer Portal. Nếu bạn đổi tên hoặc sửa đổi các tài nguyên này (Load Balancer, Volumes) trong vServer Portal, bạn có thể khiến chúng không sử dụng được cho Cluster hoặc khiến cho tài nguyên trên vServer Portal và tài nguyên trên Cluster không được đồng nhất. Để tránh điều này, hãy quản lý tài nguyên cluster của bạn với kubectl.
