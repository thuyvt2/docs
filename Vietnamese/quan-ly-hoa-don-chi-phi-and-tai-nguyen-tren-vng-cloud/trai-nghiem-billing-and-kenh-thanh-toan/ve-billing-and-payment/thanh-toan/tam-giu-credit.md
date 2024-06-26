# Tạm giữ Credit

Tại VNG Cloud Service, có một số tài nguyên / dịch vụ đặc thù chỉ dành cho người dùng trả sau vì một số lý do sau:

* Không thể tính giá thành sử dụng tài nguyên ngay tại thời điểm phát sinh
* Giá tiền sử dụng tài nguyên chỉ có thể được tính dựa trên sử dụng thực tế của người dùng.

Do đó, người dùng trả trước không thể thực hiện thanh toán chi phí để sử dụng các tài nguyên đặc biệt (có thể kể đến như dịch vụ vContainer) này ngay tại thời điểm khởi tạo.&#x20;

Nhận thấy nhu cầu sử dụng các tài nguyên / dich vụ trả sau của người dùng trả trước tăng cao, VNG Cloud Service cho ra đời tính năng **Tạm giữ Credit:**

* **Đối tượng áp dụng**:
  * Người dùng trả trước
* **Tài nguyên / Dịch vụ áp dụng**:&#x20;
  * Hiên tại đang hỗ trợ vContainer (K8s) và Snapshot
* **Nguồn tiền**:
  * VNG Cloud Credit
* **Tác vụ:**
  * Người dùng chọn cấu hình tài nguyên cần sử dụng
  * Xác nhận thực hiện tạm giữ credit tại trang thanh toán
* **Kết quả khi hoàn thành tác vụ:**
  * Hệ thống tiến hành tạm giữ credit: Số credit bị tạm giữ người dùng sẽ không được sử dụng cho mục đích khác
  * Hệ thống cung cấp tài nguyên theo cấu hình cho người dùng
  * Gửi mail thông báo thông tin tài nguyên (tùy vào đặc thù từng loại tài nguyên mà sẽ có email thông báo hoặc không)

#### 1. Tạm giữ credit dịch vụ vContainer (K8s) <a href="#tamgiucredit-1.tamgiucreditdichvuvcontainer-k8s" id="tamgiucredit-1.tamgiucreditdichvuvcontainer-k8s"></a>

Sau khi khởi tạo tài nguyên thành công, cứ mỗi cuối ngày, hệ thống sẽ tự động tính lại số tiền sử dụng thực tế của ngày hôm đó và tiến hành tạm giữ số credit cần có để sử dụng dịch vụ trong 3 ngày tiếp theo. Tham khảo ví dụ sau:

| Tác vụ                                          | Ước tính số tiền sử dụng dịch vụ trong 3 ngày (1) | Số dư credit khả dụng thực tế (2) | Chi phí sử dụng thực tế mỗi ngày (3) | Số credit cần đặt cọc (4)                        | Số Credit khả dụng còn lại khi thực hiện tác vụ (5) |
| ----------------------------------------------- | ------------------------------------------------- | --------------------------------- | ------------------------------------ | ------------------------------------------------ | --------------------------------------------------- |
| Khởi tạo dịch vụ (ngày n) với 2 node 4 volumes  | 1,800,000 VND                                     | 50,000,000 VND                    | N/A                                  | 1,800,000 VND                                    | 50,000,000 - 1,800,000 = 48,200,000 VND             |
| Ngày n+1 với cấu hình không thay đổi            | 1,800,000 VND                                     | 48,200,000 VND                    | 600,000 VND                          | 600,000 + 1,800,000 = 2,400,000 VND              | 50,000,000 - 2,400,000 = 47,600,000 VND             |
| Ngày n+2, cấu hình không thay đổi               | 1,800,000 VND                                     | 47,600,000 VND                    | 600,000 VND                          | 600,000\*2 + 1,800,000 = 3,000,000 VND           | 50,000,000 - 3,000,000 = 47,000,000 VND             |
| Ngày n+3, scale up lên 3 node 6 volume          | 2,700,000 VND                                     | 47,000,000 VND                    | 600,000 VND                          | 600,000\*3 + 2,700,000 = 4,500,000 VND           | 50,000,000 - 4,500,000 = 45,500,000 VND             |
| Ngày n + 4, giữ nguyên cấu hình 3 node 6 volume | 2,700,000 VND                                     | 45,500,000 VND                    | 900,000 VND                          | 600,000\*3 + 900,000 + 2,700,000 = 5,400,000 VND | 50,000,000 - 5,400,000 = 44,600,000 VND             |
| Ngày n + 5, xóa cluster, không sử dụng nữa      | 0                                                 | 44,600,000 VND                    | 900,000 VND                          | 600,000\*3 + 900,000\*2 = 3,600,000 VND          | 50,000,000 - 3,600,000 = 46,400,000 VND             |

**Giải thích công thức tính:**

* (1) Số tiền ước tính khi sử dụng dịch vụ trong 3 ngày liên tiếp, dựa trên cấu hình tài nguyên hiện tại
* (2) Số dư credit khả dụng đang có
* (3) Chi phí sử dụng thực tế: Chi phí chính xác cho một ngày sử dụng dịch vụ, dựa trên sự thay đổi cấu hình (nếu có)
* (4) Số credit cần đặt cọc, đây sẽ là con số cộng dồn cho các ngày sử dụng dịch vụ thực tế, cộng với chi phí ước tính cho 3 ngày tiếp theo, tại kì thanh toán hằng tháng, số tiền này sẽ được dùng cho mục đích thanh toán hóa đơn dịch vụ.
* (5) Số credit khả dụng còn lại sau khi thực hiện tác vụ tạm giữ credit = Tổng số credit khả dụng ban đầu - Tổng chi phí sử dụng thực tế (tính từ ngày khởi tạo dịch vụ đến hiện tại) - Ước tính sử dụng dịch vụ trong 3 ngày tiếp theo (dựa trên cấu hình hiện tại).

Lưu ý (\*)

* Chi phí sử dụng tài nguyên sẽ được tính chính xác đến phút, tại thời điểm người dùng thực hiện hành động (khởi tạo, thay đổi cấu hình), trên đây chỉ là ví dụ mang tính chất tham khảo.
* Khi người dùng thực hiện xóa tài nguyên K8s, hệ thống sẽ giữ lại con số sử dụng thực tế (3,600,000 VND như trên ví dụ), con số này sẽ được dùng để thanh toán hóa đơn vào kì thanh toán hằng tháng

#### 2. Tạm giữ credit dịch vụ Snapshot <a href="#tamgiucredit-2.tamgiucreditdichvusnapshot" id="tamgiucredit-2.tamgiucreditdichvusnapshot"></a>

Lưu ý rằng, tại thời điểm kích hoạt dịch vụ Snapshot lần đầu tiên, người dùng sẽ không bị tính bất kì một khoản chi phí nào. Việc tạm giữ credit chỉ xảy ra mỗi ngày một lần, trong trường hợp tài khoản sử dụng có khởi tạo và lưu trữ các bản dữ liệu Snapshot. Tham khảo hướng dẫn bên dưới để hiểu thêm về cách triển khai tạm giữ credit đối với dịch vụ Snpashot:

**Cách tính phí Snapshot**

* **Công thức**: Chi phí của việc tạo bản snapshot được tính dựa trên kích thước của bản snapshot (theo đơn vị gigabyte) và thời gian sử dụng của nó (thường được đo bằng giờ).
* **Thời gian sử dụng**: Người dùng sẽ được tính phí cho thời gian tồn tại của bản snapshot. Thời gian này được ghi nhận theo giờ.
* **Ví dụ**: Ví dụ, nếu bạn có một bản snapshot có kích thước 100GB và giá đơn vị cho Dịch Vụ Snapshot là 7,7 VND cho mỗi GB-giờ, thì chi phí sẽ được tính như sau:
  * Theo giờ: 7,7 VND \* 100 GB = 770 VND mỗi giờ.
  * Theo ngày: 770 VND \* 24 = 18,480 VND mỗi ngày.
  * Theo tháng: 18,480 \* 30 = 554,400 VND mỗi tháng.
* **Lưu ý**: Giá đơn vị được cung cấp chỉ để tham khảo. Thời gian sử dụng thực tế của các bản snapshot được ghi nhận hàng giờ, dựa trên kích thước thực tế của các bản snapshot của bạn.

**Ví dụ cụ thể**

* Bước 1: Kích hoạt Snapshot lần đầu tiên (Không mất phí). Ví dụ Kích hoạt lúc 9am
* Bước 2: Khởi tạo và lưu trữ các bản Snapshot theo nhu cầu sử dụng. Ví dụ như sau:
  * 10am khởi tạo Snapshot dung lượng 10GB
  * 1pm khởi tạo thêm 10GB nữa, total Snapshot Size 20GB
* Bước 3: Hệ thống ghi nhận dung lượng sử dụng Snapshot mỗi giờ 1 lần
* Bước 4: Tạm giữ Credit mỗi ngày 1 lần dựa trên sử dụng thực tế. Cụ thể như sau:
  * Thời gian chạy của hệ thống: 9am ngày hôm sau
  * 10GB Snapshot Size cho 3 giờ sử dụng (từ 10am đến 1pm) = 7,7 \* 10 \* 3 = 231 VND
  * 20GB Snapshot Size cho 20 giờ sử dụng (từ 1pm đến 9am hôm sau) = 7,7 \* 20 \* 20 = 3,080 VND
  * Tổng số tiền sử dụng thực tế cho 1 ngày = 231 VND + 3,080 VND = 3,311 VND
  * _Tổng số tiền cần tạm giữ = Số tiền sử dụng thực tế + Ước tính sử dụng cho 3 ngày tiếp theo = 3,311 + (7,7 VND \* 20GB \* 24h \* 3 days) **(\*)** = 14,399 VND_

Lưu ý (\*)

* Hệ thống cần tạm giữ phần sử dụng thực tế và cả phần dự đoán sử dụng cho 3 ngày tiếp theo để đảm bảo tính an toàn và xuyên suốt khi sử dụng dịch vụ.
* Công thức ước tính sử dụng cho 3 ngày tiếp theo được tính như sau: **TotalSnapshotSize \* Đơn giá VND \* 24h \* 3 days**

#### 3. Không đủ số dư credit khả dụng để tạm giữ credit <a href="#tamgiucredit-3.khongdusoducreditkhadungdetamgiucredit" id="tamgiucredit-3.khongdusoducreditkhadungdetamgiucredit"></a>

Trường hợp người dùng không đủ số dư credit khả dụng sau khi thực hiện việc đặt cọc credit, hệ thống sẽ:

* Gửi mail thông báo đến người dùng số thông tin tài nguyên và credit.

Người dùng chủ động nạp thêm credit để tránh việc nợ credit chạm ngưỡng cho phép, gây ảnh hưởng đến quá trình sử dụng dịch vụ
