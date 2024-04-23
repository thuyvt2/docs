# Ingress for an Application Load Balancer

Để cho tài nguyên Ingress (Ingress Yaml file) có thể hoạt động được, cluster phải có 1 VNGCloud Ingress Controller đang chạy. Không giống các loại Controller khác được chạy như là 1 phần của **kube-controller-manager**. VNGCloud Ingress Controller không được tự động khởi động cùng với cluster. Hãy làm theo hướng dẫn sau đây để cài đặt VNGCloud Ingress Controller cũng như làm việc với Ingress Yaml file.

### Chuẩn bị <a href="#ingressforanapplicationloadbalancer-chuanbi" id="ingressforanapplicationloadbalancer-chuanbi"></a>

* Tạo một Kubernetes cluster trên VNGCloud, hoặc sử dụng một cluster đã có. Lưu ý: đảm bảo bạn đã tải xuống cluster configuration file sau khi cluster được khởi tạo thành công và truy cập vào cluster của bạn.
* Khởi tạo hoặc sử dụng một **service account** đã tạo trên IAM và gắn policy: **vLBFullAccess**, **vServerFullAccess**. Để tạo service account bạn truy cập tại [đây](https://hcm-3.console.vngcloud.vn/iam/service-accounts) và thực hiện theo các bước sau:
  * Chọn "**Create a Service Account**", điền tên cho Service Account và nhấn **Next Step** để gắn quyền cho Service Account
  * Tìm và chọn **Policy:** **vLBFullAccess và Policy:** **vServerFullAccess**, sau đó nhấn "**Create a Service Account**" để tạo Service Account, Policy: vLBFullAccess vàPolicy: vServerFullAccess do VNG Cloud tạo ra, bạn không thể xóa các policy này.
  * Sau khi tạo thành công bạn cần phải lưu lại **Client\_ID** và **Secret\_Key** của Service Account để thực hiện bước tiếp theo.
* Thay đổi thông tin **Security Group** để cho phép các ALB có thể kết nối được tới các Node trong Node Group của bạn. Bạn cần thay đổi chúng trên vServer Portal khi:
  * Security Group được gắn vào **Cluster/Node Group** của bạn **khác với thông số mặc định** mà chúng tôi đã tạo.
  * Bạn cần **thay đổi mức độ bảo mật** cho Cluster của mình hoặc bạn cần **mở thêm cổng** cho các dịch vụ cụ thể hoạt động trên Cluster. Chi tiết tham khảo tại [đây.](https://docs.vngcloud.vn/display/vServer/Security+Groups)

***

### Khởi tạo Service Account và cài đặt VNGCloud Ingress Controller <a href="#exposemotservicethongquavlblayer7-khoitaoserviceaccountvacaidatvngcloudingresscontroller" id="exposemotservicethongquavlblayer7-khoitaoserviceaccountvacaidatvngcloudingresscontroller"></a>

{% hint style="info" %}
Chú ý:

Khi bạn thực hiện khởi tạo Cluster theo hướng dẫn bên trên, nếu bạn chưa bật option **Enable vLB Native Integration Driver**, mặc định chúng tôi sẽ không cài sẵn plugin này vào Cluster của bạn. Bạn cần tự thực hiện Khởi tạo Service Account và cài đặt VNGCloud Ingress Controller theo hướng dẫn bên dưới. Nếu bạn đã bật option **Enable vLB Native Integration Driver**, thì chúng tôi đã cài sẵn plugin này vào Cluster của bạn, hãy bỏ qua bước Khởi tạo Service Account, cài đặt VNGCloud Ingress Controller và tiếp tục thực hiện theo hướng dẫn kể từ Deploy một Workload.
{% endhint %}

<details>

<summary>Khởi tạo Service Account và cài đặt VNGCloud Ingress Controller</summary>

#### Khởi tạo Service Account <a href="#exposemotservicethongquavlblayer7-khoitaoserviceaccount" id="exposemotservicethongquavlblayer7-khoitaoserviceaccount"></a>

* Khởi tạo hoặc sử dụng một **service account** đã tạo trên IAM và gắn policy: **vLBFullAccess**, **vServerFullAccess**. Để tạo service account bạn truy cập tại [đây](https://hcm-3.console.vngcloud.vn/iam/service-accounts) và thực hiện theo các bước sau:
  * Chọn "**Create a Service Account**", điền tên cho Service Account và nhấn **Next Step** để gắn quyền cho Service Account
  * Tìm và chọn **Policy:** **vLBFullAccess và Policy:** **vServerFullAccess**, sau đó nhấn "**Create a Service Account**" để tạo Service Account, Policy: vLBFullAccess vàPolicy: vServerFullAccess do VNG Cloud tạo ra, bạn không thể xóa các policy này.
  * Sau khi tạo thành công bạn cần phải lưu lại **Client\_ID** và **Secret\_Key** của Service Account để thực hiện bước tiếp theo.

#### Cài đặt VNGCloud Ingress Controller <a href="#exposemotservicethongquavlblayer7-caidatvngcloudingresscontroller" id="exposemotservicethongquavlblayer7-caidatvngcloudingresscontroller"></a>

* Cài đặt Helm phiên bản từ 3.0 trở lên. Tham khảo tại [https://helm.sh/docs/intro/install/](https://helm.sh/docs/intro/install/) để biết cách cài đặt.
* Thêm repo này vào cluster của bạn qua lệnh:

```
helm repo add vks-helm-charts https://vngcloud.github.io/vks-helm-charts
helm repo update
```

* Thay thế thông tin ClientID, Client Secret và ClusterID của cụm K8S của bạn và tiếp tục chạy:

```
helm install vngcloud-ingress-controller vks-helm-charts/vngcloud-ingress-controller \
  --namespace kube-system \
  --set cloudConfig.global.clientID= <Lấy ClientID của Service Account được tạo trên IAM theo hướng dẫn bên trên> \
  --set cloudConfig.global.clientSecret= <Lấy ClientSecret của Service Account được tạo trên IAM theo hướng dẫn bên trên>\
  --set cluster.clusterID= <Lấy Cluster ID của cluster mà bạn đã khởi tạo trước đó>
```

* Sau khi việc cài đặt hoàn tất, thực hiện kiểm tra trạng thái của vngcloud-ingress-controller pods:

```
kubectl get pods -n kube-system | grep vngcloud-ingress-controller
```

Ví dụ như ảnh bên dưới là bạn đã cài đặt thành công vngcloud-controller-manager:

```
NAME                                      READY   STATUS    RESTARTS   AGE
vngcloud-ingress-controller-0             1/1     Running   0          12s
```

</details>

***

### Deploy một Workload <a href="#exposemotservicethongquavlblayer7-deploymotworkload" id="exposemotservicethongquavlblayer7-deploymotworkload"></a>

Sau đây là hướng dẫn để bạn deploy service nginx trên Kubernetes.

**Bước 1**: **Tạo Deployment cho Nginx app.**

* Tạo file **nginx-service.yaml** với nội dung sau:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-app
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 1
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.19.1
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx 
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

* Deploy Deployment này bằng lệch:&#x20;

```
kubectl apply -f nginx-service.yaml
```

***

**Bước 2: Kiểm tra thông tin Deployment, Service vừa deploy**

* Chạy câu lệnh sau đây để kiểm tra **Deployment**

```
kubectl get svc,deploy,pod -owide
```

* Nếu kết quả trả về như bên dưới tức là bạn đã deploy Deployment thành công.

```
NAME                    TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)           AGE     SELECTOR
service/kubernetes      ClusterIP      10.96.0.1       <none>        443/TCP           2d4h    <none>
service/nginx-app       NodePort       10.96.215.192   <none>        30080:31289/TCP   6m12s   app=nginx
service/nginx-service   LoadBalancer   10.96.179.221   <pending>     80:32624/TCP      16s     app=nginx

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES         SELECTOR
deployment.apps/nginx-app   1/1     1            1           16s   nginx        nginx:1.19.1   app=nginx

NAME                             READY   STATUS    RESTARTS   AGE   IP              NODE                                            NOMINATED NODE   READINESS GATES
pod/nginx-app-7f45b65946-t7d7k   1/1     Running   0          16s   172.16.24.202   ng-3f06013a-f6a5-47ba-a51f-bc5e9c2b10a7-ecea1   <none>           <none>
```

***

### **Tạo Ingress Resource**

#### **1.Nếu bạn chưa có sẵn một Application Load Balancer nào** đã khởi tạo trước đó trên hệ thống vLB. <a href="#ingressforanapplicationloadbalancer-1.neubanchuacosanmotapplicationloadbalancernaodakhoitaotruocdotr" id="ingressforanapplicationloadbalancer-1.neubanchuacosanmotapplicationloadbalancernaodakhoitaotruocdotr"></a>

Lúc này, khi tạo một Ingress, bạn hãy để trống thông tin Load Balancer ID tại annotation [vks.vngcloud.vn/load-balancer-id](http://vks.vngcloud.vn/load-balancer-id).

* Ví dụ, giả sử bạn đã deployment một service có tên nginx-service. Lúc này, bạn có thể tạo file **nginx-ingress.yaml** như sau:

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
spec:
  ingressClassName: "vngcloud"
  defaultBackend:
    service:
      name: nginx-service
      port:
        number: 80
  rules:
    - http:
        paths:
          - path: /path1
            pathType: Exact
            backend:
              service:
                name: nginx-service
                port:
                  number: 80               
```

* Chạy câu lệnh sau đây để triển khai Ingress

```
kubectl apply -f nginx-ingress.yaml
```

Sau khi bạn đã thực hiện triển khai Ingress , Chúng tôi sẽ tự động tạo 1 ALB trên cluster của bạn. ALB này sẽ hiển thị trên vLB Portal, chi tiết truy cập tại [đây.](https://hcm-3.console.vngcloud.vn/vserver/load-balancer/vlb) ALB này sẽ có thông tin mặc định:&#x20;

<table data-header-hidden><thead><tr><th width="177"></th><th width="120"></th><th></th></tr></thead><tbody><tr><td><strong>Thành phần</strong></td><td><strong>Số lượng</strong></td><td><strong>Thuộc tính</strong></td></tr><tr><td>ALB Package </td><td>1</td><td>VNG ALB_Small</td></tr><tr><td>Listener</td><td>2</td><td><ul><li>1 listener với protocol HTTP và port 80</li><li>1 listener với protocol HTTPS và port 443</li></ul></td></tr><tr><td>Pool</td><td>1</td><td><ul><li>1 pool default protocol HTTP và algorithm ROUND ROBIN</li></ul></td></tr><tr><td>Health Check</td><td>1</td><td><ul><li>Sử dụng TCP để health check các member.</li></ul></td></tr></tbody></table>

Ví dụ:&#x20;

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Chú ý:&#x20;

* Hiện tại Ingress chỉ hỗ trợ duy nhất TLS port 443 và là điểm kết thúc cho TLS (TLS termination). TLS Secret phải chứa các trường với tên key là tls.crt và tls.key, đây chính là certificate và private key để sử dụng cho TLS. Nếu bạn muốn sử dụng Certificate cho một host, hãy thực hiện tải lên Certificate theo hướng dẫn tại [Upload a certificate](https://docs-admin.vngcloud.vn/display/vServer/Upload+a+certificate) và sử dụng chúng như một annotation. Ví dụ:&#x20;

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    # kubernetes.io/ingress.class: "vngcloud" # this annotation is deprecated will cause warning, can use option `ingressClassName` below instead
    vks.vngcloud.vn/certificate-ids: "secret-a6d20ec6-f3e5-499a-981b-b1484e340cec"
spec:
  ingressClassName: "vngcloud"
  defaultBackend:
    service:
      name: apache-service
      port:
        number: 80
  tls:
    - hosts:
        - host.example.com
  rules:
    - host: host.example.com
      http:
        paths:
          - path: /nginx
            pathType: Exact
            backend:
              service:
                name: nginx-service
                port:
                  number: 80
```
{% endhint %}

***

#### **2.Nếu bạn đã có sẵn một Application Load Balancer** đã khởi tạo trước đó trên hệ thống vLB và bạn muốn tái sử dụng ALB cho cluster của bạn. <a href="#ingressforanapplicationloadbalancer-2.neubandacosanmotapplicationloadbalancerdakhoitaotruocdotrenhet" id="ingressforanapplicationloadbalancer-2.neubandacosanmotapplicationloadbalancerdakhoitaotruocdotrenhet"></a>

Lúc này, khi tạo một Ingress, bạn hãy nhập thông tin Load Balancer ID vào annotation [vks.vngcloud.vn/load-balancer-id](http://vks.vngcloud.vn/load-balancer-id). Ví dụ, trong trường hợp này tôi đã tái sử dụng ALB có ID = lb-2b9d8974-3760-4d60-8203-9671f229fb96:

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    # kubernetes.io/ingress.class: "vngcloud" # this annotation is deprecated will cause warning, can use option `ingressClassName` below instead.
    vks.vngcloud.vn/load-balancer-id: "lb-6cdea8fd-4589-410e-933f-c3bc46fa9d25"
    vks.vngcloud.vn/certificate-ids: "secret-a6d20ec6-f3e5-499a-981b-b1484e340cec"
spec:
  ingressClassName: "vngcloud"
  defaultBackend:
    service:
      name: apache-service
      port:
        number: 80
  tls:
    - hosts:
        - host.example.com
  rules:
    - host: host.example.com
      http:
        paths:
          - path: /path1
            pathType: Exact
            backend:
              service:
                name: nginx-service
                port:
                  number: 80
```

* Sau khi bạn đã thực hiện tạo ingress theo hướng dẫn tại [Ingress for an Application Load Balancer](https://docs.vngcloud.vn/display/VKSVI/Ingress+for+an+Application+Load+Balancer). Nếu:&#x20;
  * ALB của bạn đang có sẵn 2 listener trong đó:&#x20;
    * 1 listener có cấu hình protocol HTTP và port 80
    * 1 listener có cấu hình protocol HTTPS và port 443 thì chúng tôi sẽ sử dụng 2 listener này.
  * ALB của bạn chưa có một trong hai hoặc cả 2 listener có cấu hình trên, chúng tôi sẽ tự động khởi tạo chúng.

{% hint style="info" %}
Chú ý:&#x20;

Nếu ALB của bạn có:

* 1 listener có cấu hình protocol HTTP và port 443
* Hoặc 1 listener có cấu hình protocol HTTPS và portal 80

thì khi tạo Ingress sẽ xảy ra lỗi. Lúc này, bạn cần chỉnh sửa lại thông tin listener hợp lệ trên hệ thống vLB và thực hiện tạo lại ingress.
{% endhint %}

***

#### **3. Sau khi tạo ingress thành công với một ALB**, bạn có thể thực hiện <a href="#ingressforanapplicationloadbalancer-3.saukhitaoingressthanhcongvoimotalb-bancothethuchien" id="ingressforanapplicationloadbalancer-3.saukhitaoingressthanhcongvoimotalb-bancothethuchien"></a>

* Chỉnh sửa cấu hình ingress của bạn theo hướng dẫn cụ thể tại [Configure for an Application Load Balancer](https://docs.vngcloud.vn/display/VKSVI/Configure+for+an+Application+Load+Balancer).
* Hoặc bạn có thể thêm/ sửa/ xóa policy trong ALB của bạn bằng các chỉnh sửa các thông số sau trong tài nguyên ingress (Ingress Yaml file). Ví dụ như bên dưới, tôi đã thực hiện thiết lập **2 rule** như sau:

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
  annotations:
    vks.vngcloud.vn/certificate-ids: "secret-58542bfb-f410-4095-9e1c-34cd39995c6d, secret-893b67e2-0286-434d-9e11-a8af997aefb8"
spec:
  ingressClassName: "vngcloud"
  defaultBackend:
    service:
      name: nginx-service
      port:
        number: 80
  tls:
    - hosts:
       - '*.example.com'
       - 'example.com'
  rules:
    - host: example.com
      http:
        paths:
          - path: /path1
            pathType: Exact
            backend:
              service:
                name: nginx-service1
                port:
                  number: 80
    - http:
        paths:
          - path: /path2
            pathType: Exact
            backend:
              service:
                name: nginx-service2
                port:
                  number: 80
```

* Cũng giống như các tài nguyên Kubernetes khác, Ingress có cấu trúc gồm các trường thông tin như sau:
  * **apiVersion:** Phiên bản API cho Ingress.
  * **kind:** Loại tài nguyên, trong trường hợp này là "Ingress".
  * **ingressClassName**: bạn cần chỉ định giá trị field này là "vngcloud" để sử dụng vngcloud-ingress-controller.
  * **metadata:** Thông tin mô tả Ingress, bao gồm tên, annotations.&#x20;
  * **spec:** Cấu hình Ingress, bao gồm các rule route traffic theo điều kiện của các incoming request. Tài nguyên Ingress chỉ hỗ trợ các rule để điều hướng HTTP traffic.

Để biết thông tin chung về cách làm việc với tài nguyên Ingress (Ingress Yaml file), hãy xem tại [Configure for an Application Load Balancer](https://docs-admin.vngcloud.vn/display/VKSVI/Configure+for+an+Application+Load+Balancer).

***

### Kiểm tra, chỉnh sửa Ingress resource đã tạo <a href="#ingressforanapplicationloadbalancer-trienkhaiingress" id="ingressforanapplicationloadbalancer-trienkhaiingress"></a>

* Sau khi tạo ingress thành công, bạn có thể xem danh sách ingress qua lệnh

```
kubectl get ingress
```

Ví dụ, bên dưới chúng tôi đã tạo thành công nginx-ingress:

```
NAME            CLASS      HOSTS   ADDRESS          PORTS   AGE
nginx-ingress   vngcloud   *       180.93.181.129   80      103m
```

* Hoặc xem chi tiết một ingress bằng cách

```
kubectl describe ingress nginx-ingress
```

Ví dụ, bên dưới là thông tin chi tiết của nginx-ingress mà tôi đã tạo:

```
Name:             nginx-ingress
Labels:           <none>
Namespace:        default
Address:          180.93.181.129
Ingress Class:    vngcloud
Default backend:  nginx-service:80 (172.16.24.202:80)
Rules:
  Host        Path  Backends
  ----        ----  --------
  *
              /path1   nginx-service:80 (172.16.24.202:80)
Annotations:  vks.vngcloud.vn/load-balancer-name: vks-k8s-6d2acb-default-nginx-ingr-897e5
Events:       <none>
```

*   Để cập nhật nginx-ingress hiện có, ta có thể thực hiện bằng cách cập nhật Ingress Yaml file như sau:

    ```
    kubectl edit ingress nginx-ingress
    ```

***

**Để truy cập vào app nginx, bạn có thể sử dụng Endpoint của Load Balancer mà hệ thống đã tạo.**&#x20;

```
http://Endpoint/
```

Bạn có thể lấy thông tin Public Endpoint của Load Balancer tại giao diện vLB. Cụ thể truy cập tại&#x20;

Ví dụ, bên dưới tôi đã truy cập thành công vào app nginx với địa chỉ : [http://180.93.181.129/](http://180.93.181.129/)

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>
