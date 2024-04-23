# Configure for an Application Load Balancer

Tại trang [Ingress for an Application Load Balancer](https://docs-admin.vngcloud.vn/display/VKSVI/Ingress+for+an+Application+Load+Balancer), chúng tôi đã hướng dẫn bạn cách thực hiện cài đặt Ingress Controller và tạo ingress thông qua Ingress Yaml file. Sau đây là chi tiết các ý nghĩa các thông tin bạn có thể thiết lập cho một Ingress

### Annotation <a href="#configureforanapplicationloadbalancer-annotation" id="configureforanapplicationloadbalancer-annotation"></a>

Sử dụng các annotation dưới đây khi thực thiện tạo ingress để tuỳ chỉnh Load Balancer phù hợp với nhu cầu của bạn:

<table><thead><tr><th width="129">Annotation</th><th width="230">Bắt buộc/ Không bắt buộc</th><th>Ý nghĩa</th></tr></thead><tbody><tr><td><a href="http://vks.vngcloud.vn/load-balancer-id">/load-balancer-id</a></td><td>Không bắt buộc</td><td><ul><li><strong>Nếu bạn chưa có sẵn một Application Load Balancer</strong> đã khởi tạo trước đó trên hệ thống vLB. Lúc này, khi tạo một Ingress, bạn hãy để trống thông tin này. Sau khi bạn đã thực hiện triển khai Ingress theo hướng dẫn tại <a href="https://docs.vngcloud.vn/display/VKSVI/Ingress+for+an+Application+Load+Balancer">Ingress for an Application Load Balancer</a>. Chúng tôi sẽ tự động tạo 1 ALB trên cluster của bạn. ALB này sẽ hiển thị trên vLB Portal, chi tiết truy cập tại <a href="https://hcm-3.console.vngcloud.vn/vserver/load-balancer/vlb">đây</a></li><li><p><strong>Nếu bạn đã có sẵn một Application Load Balancer</strong> đã khởi tạo trước đó trên hệ thống vLB và bạn muốn tái sử dụng ALB cho cluster của bạn. Lúc này, khi tạo một Ingress, bạn hãy nhập thông tin Load Balancer ID vào annotation này. Sau khi bạn đã thực hiện tạo Ingress theo hướng dẫn tại <a href="https://docs.vngcloud.vn/display/VKSVI/Ingress+for+an+Application+Load+Balancer">Ingress for an Application Load Balancer</a>. Nếu: </p><ul><li><p>ALB của bạn đang có sẵn 2 listener trong đó: </p><ul><li>1 listener có cấu hình protocol HTTP và port 80</li><li>1 listener có cấu hình protocol HTTPS và port 443 thì chúng tôi sẽ sử dụng 2 listener này.</li></ul></li><li>ALB của bạn chưa có một trong hai hoặc cả 2 listener có cấu hình trên, chúng tôi sẽ tự động khởi tạo chúng.</li></ul></li></ul><p>Chú ý:</p><p>Nếu ALB của bạn có:</p><ul><li>1 listener có cấu hình protocol HTTP và port 443</li><li>Hoặc 1 listener có cấu hình protocol HTTPS và portal 80</li></ul><p>thì khi tạo Ingress sẽ xảy ra lỗi. Lúc này, bạn cần chỉnh sửa lại thông tin listener hợp lệ trên hệ thống vLB và thực hiện tạo lại ingress.</p></td></tr><tr><td><a href="http://vks.vngcloud.vn/package-id">/package-id</a></td><td>Không bắt buộc</td><td><ul><li>Nếu bạn không nhập thông tin này thì mặc định chúng tôi sẽ sử dụng cấu hình <strong>ALB Small.</strong></li><li>Nếu bạn đã có sẵn host vLB đang ACTIVE và bạn muốn tích hợp host này vô cụm K8S của bạn, vui lòng bỏ qua trường thông tin này.</li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#tags">/tags</a></td><td>Không bắt buộc</td><td><ul><li>Tag được gắn thêm cho ALB của bạn.</li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#scheme">/scheme</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>internet-facing</strong>, bạn có thể đổi thành <strong>internal</strong> tùy theo nhu cầu sử dụng.</li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#security-groups">/security-groups</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định sẽ tạo một <strong>security group default</strong> theo Cluster của bạn.</li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#inbound-cidrs">/inbound-cidrs</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định All CIRD: <strong>0.0.0.0/0</strong></li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#healthy-threshold-count">/healthy-threshold-count</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>3</strong></li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#unhealthy-threshold-count">/unhealthy-threshold-count</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>3</strong></li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#healthcheck-interval-seconds">/healthcheck-interval-seconds</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>30</strong></li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#healthcheck-timeout-seconds">/healthcheck-timeout-seconds</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>5</strong></li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#healthcheck-protocol">/healthcheck-protocol</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>TCP</strong>. Người dùng có thể chọn một trong các giá trị TCP/ HTTP</li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#healthcheck-http-method">/healthcheck-http-method</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>GET</strong>. Người dùng có thể chọn một trong các giá trị GET / POST / PUT</li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#healthcheck-path">/healthcheck-path</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>/</strong></li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#healthcheck-http-version">/healthcheck-http-version</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>1.0</strong>. Người dùng có thể chọn một trong các giá trị 1.0, 1.1</li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#healthcheck-http-domain-name">/healthcheck-http-domain-name</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định trống</li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#healthcheck-port">/healthcheck-port</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>traffic port</strong></li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#success-codes">/success-codes</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>200</strong></li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#idle-timeout-client">/idle-timeout-client</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>50</strong></li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#idle-timeout-member">/idle-timeout-member</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>50</strong></li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#idle-timeout-connection">/idle-timeout-connection</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>5</strong></li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#pool-algorithm">/pool-algorithm</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>ROUND_ROBIN</strong>. Người dùng có thể chọn một trong các giá trị ROUND_ROBIN / LEAST_CONNECTIONS / SOURCE_IP</li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#enable-sticky-session">/enable-sticky-session</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>false</strong>.</li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#enable-tls-encryption">/enable-tls-encryption</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định <strong>false</strong></li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#target-node-labels">/target-node-labels</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định trống</li></ul></td></tr><tr><td><a href="https://anngdinh.github.io/vcontainer-helm-infra-documentation/helm-charts/vngcloud-ingress-controller/annotations.html#certificate-ids">/certificate-ids</a></td><td>Không bắt buộc</td><td><ul><li>Mặc định trống</li></ul></td></tr></tbody></table>

***

### IngressClassName <a href="#configureforanapplicationloadbalancer-ingressclassname" id="configureforanapplicationloadbalancer-ingressclassname"></a>

Các Ingress được cài đặt bởi các VNGCloud Ingress Controller sẽ có thông tin IngressClassName = "vngcloud". Bạn không được thay đổi thông tin này.

***

### DefaultBackend

* Một Ingress không có rule nào sẽ gửi tất cả traffic đến 1 service default backend mặc định duy nhất hoặc nếu không có _host_ và _path_ nào khớp với HTTP request trong Ingress Yaml file thì traffic sẽ được route đến service default backend. Ví dụ như bên dưới, chúng tôi đang cấu hình mặc định nếu request không thỏa mãn rule nào trong Ingress yaml file thì sẽ đi vào service name: example-svc-1 với port number 8080

```
defaultBackend:
    service:
      name: example-svc-1
      port:
        number: 8080
```

***

### TLS <a href="#configureforanapplicationloadbalancer-tls" id="configureforanapplicationloadbalancer-tls"></a>

Bạn có thể bảo mật Ingress bằng cách chỉ định 1 Secret có chứa TLS key và certificate. Hiện tại Ingress chỉ hỗ trợ duy nhất TLS port 443 và là điểm kết thúc cho TLS (TLS termination). TLS Secret phải chứa các trường với tên key là tls.crt và tls.key, đây chính là certificate và private key để sử dụng cho TLS.  Cụ thể, bạn cần chỉ định:

* Host: các host được chỉ định sẽ dùng cert.
* SecretName: tên secret chứa cert.

***

### Path types <a href="#configureforanapplicationloadbalancer-pathtypes" id="configureforanapplicationloadbalancer-pathtypes"></a>

Mỗi path (đường dẫn) trong Ingress có một pathType (loại đường dẫn) tương ứng. Có ba pathType được hỗ trợ:

* Exact: Khớp với đường dẫn URL một cách chính xác tuyệt đối và phân biệt chữ hoa chữ thường.
* Prefix: Khớp dựa trên tiền tố của đường dẫn URL được phân tách bởi dấu /. Việc so khớp (match) là có phân biệt chữ hoa thường và được thực hiện trên từng thành phần của đường dẫn URL. Một thành phần của đường dẫn URL chính là 1 label được phân tách bằng dấu phân cách / trong đường dẫn URL (Nghĩa là đường dẫn URL có thể bao gồm nhiều cấp phân tách nhau bởi dẫu /, mỗi chuỗi đứng giữa 2 dấu / chính là 1 label, mỗi label chính là 1 thành phần của đường dẫn URL). Một request URL được xem như là khớp với 1 trường path (được cấu hình trong đặc tả Ingress) khi toàn bộ giá trị của path (có thể gồm nhiều thành phần phân tách bằng dấu /) khớp với các label đầu tiên (tính từ bên trái của đường dẫn URL). Ví dụ /example1/path1 khớp với /example1/path1/path2, nhưng không khớp với /example1/path1path2

Ví dụ cụ thể:&#x20;

| Path type | Path(s)                             | Request path(s)           | Có match hay không? |
| --------- | ----------------------------------- | ------------------------- | ------------------- |
| Exact     | `/example1`                         | `/example1`               | Có                  |
| Exact     | `/example1`                         | `/host1`                  | Không               |
| Exact     | `/example1`                         | `/example1/`              | Không               |
| Exact     | `/example1/`                        | `/example1`               | Không               |
| Prefix    | `/`                                 | (all paths)               | Có                  |
| Prefix    | `/example1`                         | `/example1`, `/example1/` | Có                  |
| Prefix    | `/example1/`                        | `/example1`, `/example1/` | Có                  |
| Prefix    | `/example1/host11`                  | `/example1/host1`         | Không               |
| Prefix    | `/example1/host1`                   | `/example1/host1`         | Có                  |
| Prefix    | `/example1/host1/`                  | `/example1/host1`         | Có                  |
| Prefix    | `/example1/host1`                   | `/example1/host1/`        | Có                  |
| Prefix    | `/example1/host1`                   | `/example1/host1/ccc`     | Có                  |
| Prefix    | `/example1/host1`                   | `/example1/host1xyz`      | Không               |
| Prefix    | `/`, `/example1`                    | `/example1/ccc`           | Có                  |
| Prefix    | `/`, `/example1`, `/example1/host1` | `/example1/host1`         | Có                  |
| Prefix    | `/`, `/example1`, `/example1/host1` | `/ccc`                    | Có                  |
| Prefix    | `/example1`                         | `/ccc`                    | Không               |

* Trong một số trường hợp, nhiều path bên trong Ingress sẽ khớp với đường dẫn của request URL. Trong những trường hợp đó, quyền ưu tiên sẽ được trao cho path khớp dài nhất đầu tiên. Nếu hai path vẫn có độ dài khớp bằng nhau thì quyền ưu tiên sẽ theo thứ tự rule được tạo trên Ingress Yaml file.&#x20;

***

### Ingress rule <a href="#configureforanapplicationloadbalancer-ingressrule" id="configureforanapplicationloadbalancer-ingressrule"></a>

Mỗi HTTP rule chứa các thông tin sau:&#x20;

* **1 host tùy chọn**. Nếu không có host (ta có thể hiểu là 1 tên miền) nào được chỉ định nên rule sẽ được áp dụng cho tất cả HTTP traffic đi vào (inbound) địa chỉ IP đã được chỉ định. Nếu có chỉ định host (ví dụ example1.com) thì rule chỉ áp dụng cho host đó thôi.
* **1 danh sách các đường dẫn (path)** (ví dụ `/example1/host1`), mỗi path có 1 service backend gắn liền với nó được định nghĩa bởi Service Name và Port Number. Cả host và path phải khớp với nội dung của incoming request trước khi bộ cân bằng tải điều hướng traffic đến Services mong muốn.
* **1 backend** là tổ hợp của tên Services và Port Number. HTTP và HTTPS request đi đến Ingress và có URL khớp với host và path của rule sẽ được gửi đến danh sách các backend.

Ví dụ: Host có khớp với Host header theo bảng:

| Host                                       | Host header                                                      | Có match hay không? |
| ------------------------------------------ | ---------------------------------------------------------------- | ------------------- |
| `*.`[`example1.com`](http://example1.com/) | [`example2.example1.com`](http://example2.example1.com/)         | Có                  |
| `*.`[`example1.com`](http://example1.com/) | [`baz.example2.example1.com`](http://baz.example2.example1.com/) | Không               |
| `*.`[`example1.com`](http://example1.com/) | [`example1.com`](http://example1.com/)                           | Không               |
