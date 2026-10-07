---
title: "Domain 3 Quick Recall Flashcards: 30 Active-Recall Questions for Exam Readiness"
domain: "Domain 3: Design High-Performing Architectures"
task_statement: "All Tasks (3.1, 3.2, 3.3, 3.4, 3.5)"
exam_weight: "Very High"
tags:
  - flashcards
  - active-recall
  - revision
  - quick-review
  - domain3
last_updated: 2026-03-30
---

# 🧠 Domain 3 Quick Recall: 30 Câu Hỏi Phản Xạ Nhanh

> Trở về: [[00-Domain-3-MOC]]  
> **Hướng dẫn ôn tập:** Đọc câu hỏi, tự nhẩm câu trả lời trong đầu trước khi bấm vào thẻ `<details>` để xem đáp án giải thích.

---

### Q1: Amazon S3 Express One Zone là gì? Đem lại lợi thế gì về độ trễ và chi phí so với S3 Standard?
<details>
<summary>👉 Xem đáp án</summary>
Là lớp lưu trữ đối tượng dạng <b>Directory Buckets</b> đặt tại duy nhất <b>1 Availability Zone</b>. Đem lại độ trễ nhất quán <b>dưới 10 mili-giây (Single-digit ms)</b> (nhanh gấp 10 lần S3 Standard) và chi phí gọi request <b>rẻ hơn tới 80%</b>.
</details>

---

### Q2: Giới hạn hiệu năng request mặc định của Amazon S3 Standard trên MỖI PREFIX là bao nhiêu?
<details>
<summary>👉 Xem đáp án</summary>
<b>3,500 PUT/POST/DELETE</b> và <b>5,500 GET/HEAD</b> requests mỗi giây trên mỗi Prefix.
</details>

---

### Q3: Khi nào Multipart Upload là BẮT BUỘC trên Amazon S3?
<details>
<summary>👉 Xem đáp án</summary>
Bắt buộc đối với các đối tượng có kích thước <b>lớn hơn 5 GB</b> (và được khuyến nghị mạnh mẽ cho các file từ <b>100 MB trở lên</b>).
</details>

---

### Q4: Kỹ thuật S3 Byte-Range Fetches đem lại lợi ích gì cho hiệu năng?
<details>
<summary>👉 Xem đáp án</summary>
Cho phép tải về song song nhiều đoạn byte nhỏ của một file lớn để tăng tốc độ tải, hoặc cho phép xem video trực tuyến tua nhanh (streaming seek) mà không cần tải cả file.
</details>

---

### Q5: Tính năng S3 Select giúp tăng tốc độ xử lý dữ liệu như thế nào?
<details>
<summary>👉 Xem đáp án</summary>
Cho phép chạy câu lệnh SQL đơn giản để lọc dữ liệu trực tiếp tại S3, chỉ truyền về máy chủ đúng các dòng/cột cần thiết, giúp <b>giảm tới 80% dữ liệu truyền qua mạng</b>.
</details>

---

### Q6: Điểm khác biệt lớn nhất giữa EBS `gp3` và `gp2` là gì?
<details>
<summary>👉 Xem đáp án</summary>
<code>gp3</code> cung cấp sẵn <b>3,000 IOPS và 125 MB/s miễn phí</b> và cho phép tăng IOPS/Throughput độc lập với dung lượng đĩa (rẻ hơn <code>gp2</code> 20%). Trong khi <code>gp2</code> bị trói chặt IOPS theo dung lượng (3 IOPS/GB).
</details>

---

### Q7: Ổ cứng EBS nào cung cấp hiệu năng tối đa lên tới 256,000 IOPS và 4,000 MB/s với độ trễ sub-millisecond cho Mission-Critical RDBMS?
<details>
<summary>👉 Xem đáp án</summary>
<b>Amazon EBS `io2 Block Express`</b>.
</details>

---

### Q8: Hai loại ổ đĩa EBS HDD (`st1` và `sc1`) có thể dùng làm ổ đĩa khởi động (Boot Volume) cho EC2 không?
<details>
<summary>👉 Xem đáp án</summary>
<b>TUYỆT ĐỐI KHÔNG</b>. <code>st1</code> và <code>sc1</code> chỉ dùng làm ổ lưu trữ dữ liệu bổ sung, không thể làm Boot Volume.
</details>

---

### Q9: Dữ liệu trên EC2 Instance Store có bị mất khi máy chủ EC2 bị Reboot không? Khi nào dữ liệu bị mất?
<details>
<summary>👉 Xem đáp án</summary>
Dữ liệu <b>VẪN CÒN khi Reboot OS</b>. Dữ liệu sẽ <b>BỊ MẤT TRẮNG</b> khi máy chủ bị <b>Stop</b>, <b>Terminate</b>, hoặc phần cứng máy chủ bên dưới bị hỏng.
</details>

---

### Q10: Muốn nhân đôi IOPS và Throughput trên máy chủ EC2 vượt quá giới hạn của 1 ổ EBS, cấu hình RAID nào được chọn?
<details>
<summary>👉 Xem đáp án</summary>
<b>RAID 0 (Striping)</b> trên nhiều ổ đĩa EBS.
</details>

---

### Q11: Chế độ Elastic Throughput của Amazon EFS hoạt động như thế nào?
<details>
<summary>👉 Xem đáp án</summary>
Tự động theo dõi và điều chỉnh thông lượng đọc/ghi tức thì theo nhu cầu thực tế của ứng dụng, chỉ trả tiền cho dung lượng thực tế đọc/ghi (lý tưởng cho tải bất định).
</details>

---

### Q12: Dịch vụ lưu trữ tệp nào được thiết kế cho High Performance Computing (HPC) với thông lượng hàng trăm GB/s và đồng bộ 2 chiều trực tiếp với Amazon S3?
<details>
<summary>👉 Xem đáp án</summary>
<b>Amazon FSx for Lustre</b>.
</details>

---

### Q13: Cụm máy chủ HPC chạy thuật toán tính toán phân tán cần độ trễ mạng thấp nhất và băng thông 100 Gbps, chọn loại Placement Group nào?
<details>
<summary>👉 Xem đáp án</summary>
<b>Cluster Placement Group</b> (gom tất cả máy vào cùng 1 rack trong 1 AZ duy nhất).
</details>

---

### Q14: Muốn bảo vệ 5 máy chủ Core Banking quan trọng đảm bảo mỗi máy nằm trên một giá đỡ phần cứng (Hardware Rack) độc lập hoàn toàn, chọn loại Placement Group nào? Giới hạn máy là bao nhiêu?
<details>
<summary>👉 Xem đáp án</summary>
<b>Spread Placement Group</b>. Giới hạn tối đa là <b>7 instances mỗi Availability Zone</b>.
</details>

---

### Q15: Loại Placement Group nào được thiết kế riêng cho các cụm dữ liệu lớn phân tán hàng trăm máy chủ như Hadoop HDFS, Cassandra và Kafka?
<details>
<summary>👉 Xem đáp án</summary>
<b>Partition Placement Group</b> (mỗi AZ tối đa 7 partitions).
</details>

---

### Q16: Card mạng Elastic Fabric Adapter (EFA) khác biệt gì so với ENA thông thường trong bài toán tính toán hiệu năng cao?
<details>
<summary>👉 Xem đáp án</summary>
EFA có tính năng <b>OS-bypass</b> (bỏ qua nhân hệ điều hành), cho phép các ứng dụng giao tiếp trực tiếp bộ nhớ với nhau qua giao thức <b>MPI</b> và <b>RDMA</b> với độ trễ cực thấp.
</details>

---

### Q17: Khi cấu hình AWS Lambda, làm thế nào để tăng sức mạnh tính toán CPU và băng thông mạng cho hàm?
<details>
<summary>👉 Xem đáp án</summary>
<b>Tăng dung lượng bộ nhớ RAM</b> (CPU và băng thông mạng tự động tăng theo tỷ lệ thuận với RAM; ở mức 1,769 MB RAM sẽ nhận tương đương 1 vCPU đầy đủ).
</details>

---

### Q18: Tính năng nào của AWS Lambda giúp loại bỏ hoàn toàn độ trễ khởi động nguội (Cold Starts)?
<details>
<summary>👉 Xem đáp án</summary>
<b>Provisioned Concurrency</b> (giữ môi trường thực thi luôn ở trạng thái "Ấm").
</details>

---

### Q19: Phân biệt trường hợp sử dụng của ElastiCache for Redis và ElastiCache for Memcached?
<details>
<summary>👉 Xem đáp án</summary>
- <b>Redis:</b> Cấu trúc dữ liệu phức tạp (Sorted Sets, Hashes), Multi-AZ tự động failover, lưu trữ bền vững (AOF/Snapshot), Pub/Sub, Bảng xếp hạng.
- <b>Memcached:</b> Thuần in-memory đa luồng (Multi-threaded), mở rộng ngang đơn giản, chỉ lưu key-value đơn thuần, không lưu bền vững.
</details>

---

### Q20: DynamoDB Accelerator (DAX) đem lại độ trễ bao nhiêu cho các thao tác đọc và có cần sửa code ứng dụng không?
<details>
<summary>👉 Xem đáp án</summary>
Đem lại độ trễ ở mức <b>Micro-giây</b> (sub-millisecond) và <b>KHÔNG CẦN sửa code ứng dụng</b> (tương thích 100% với DynamoDB API).
</details>

---

### Q21: Amazon Aurora Serverless v2 mở rộng tài nguyên tính toán như thế nào so với v1?
<details>
<summary>👉 Xem đáp án</summary>
v2 mở rộng <b>tức thì trong vài phần trăm giây</b> theo từng nấc nhỏ 0.5 ACU mà không gây đứt kết nối, và hỗ trợ đầy đủ <b>Multi-AZ, Read Replicas, và Global Database</b>.
</details>

---

### Q22: Amazon RDS Proxy giải quyết vấn đề gì khi kết hợp AWS Lambda với cơ sở dữ liệu quan hệ RDS?
<details>
<summary>👉 Xem đáp án</summary>
Cung cấp <b>Connection Pooling & Multiplexing</b>, gom hàng nghìn kết nối từ Lambda thành một nhóm nhỏ kết nối mở sẵn tới DB, ngăn chặn cạn kiệt kết nối và <b>giảm 66% thời gian failover</b>.
</details>

---

### Q23: Tại sao chuyển đổi file CSV/JSON sang Apache Parquet lại giúp Amazon Athena truy vấn nhanh hơn và rẻ hơn tới 90%?
<details>
<summary>👉 Xem đáp án</summary>
Vì Parquet là <b>định dạng lưu trữ theo cột (Columnar Format)</b>. Athena chỉ cần đọc đúng các cột có trong câu lệnh <code>SELECT</code> thay vì đọc toàn bộ file, giúp <b>giảm mạnh dung lượng dữ liệu quét (Data Scanned)</b>.
</details>

---

### Q24: "The Small Files Trap" trong tối ưu hóa Data Lake với Athena và AWS Glue là gì? Cách phòng tránh?
<details>
<summary>👉 Xem đáp án</summary>
Là lỗi phân vùng theo cột có độ phân tán cao (High Cardinality như <code>user_id</code>), tạo ra hàng triệu file nhỏ vài KB làm nghẽn Glue Catalog và khiến truy vấn chạy cực chậm. Cách tránh: Chỉ phân vùng theo thời gian/khu vực và gom file từ 128 MB - 512 MB.
</details>

---

### Q25: Tính năng Partition Projection trong AWS Glue Data Catalog giải quyết bài toán gì?
<details>
<summary>👉 Xem đáp án</summary>
Tự động tính toán vị trí phân vùng theo thuật toán số học, loại bỏ hoàn toàn việc phải chạy lệnh <code>MSCK REPAIR TABLE</code> hoặc chạy lại Glue Crawler tốn thời gian.
</details>

---

### Q26: Phân biệt sự khác biệt cốt lõi giữa Amazon CloudFront và AWS Global Accelerator?
<details>
<summary>👉 Xem đáp án</summary>
- <b>CloudFront:</b> Có <b>lưu bộ nhớ đệm (Caching)</b>, chỉ hỗ trợ HTTP/HTTPS/WebSocket.
- <b>Global Accelerator:</b> <b>KHÔNG CÓ CACHE</b>, cung cấp <b>2 IP tĩnh Anycast</b>, tăng tốc cho cả <b>TCP và UDP</b> qua mạng cáp quang riêng của AWS, failover < 1 phút.
</details>

---

### Q27: Khi nào nên chọn CloudFront Functions thay vì Lambda@Edge?
<details>
<summary>👉 Xem đáp án</summary>
Khi chỉ cần xử lý logic JavaScript siêu nhẹ (URL rewrite/redirect, chuẩn hóa header) với thời gian chạy <b>dưới 1 mili-giây (< 1ms)</b> tại 400+ Edge Locations với <b>chi phí rẻ hơn 6 lần</b>.
</details>

---

### Q28: Kinesis Data Streams Enhanced Fan-Out (EFO) đem lại lợi thế gì so với Standard Consumer?
<details>
<summary>👉 Xem đáp án</summary>
Cung cấp băng thông đọc <b>2 MB/giây RIÊNG BIỆT cho từng Consumer</b> trên mỗi shard qua giao thức HTTP/2 Server Push, giảm độ trễ đọc xuống <b>dưới 70 mili-giây</b>.
</details>

---

### Q29: Điểm khác biệt lớn nhất giữa Kinesis Data Streams và Kinesis Data Firehose là gì?
<details>
<summary>👉 Xem đáp án</summary>
- <b>Kinesis Data Streams:</b> Thời gian thực (Real-time mili-giây), lưu trữ 24h - 365 ngày, phải tự viết code consumer.
- <b>Kinesis Data Firehose:</b> Gần thời gian thực (Near real-time, đệm tối thiểu 60s), Serverless hoàn toàn, tự nạp thẳng vào S3/Redshift/OpenSearch.
</details>

---

### Q30: Khi nào nên dùng AWS DataSync thay vì Snowball Edge để chuyển dữ liệu lên AWS?
<details>
<summary>👉 Xem đáp án</summary>
Dùng <b>AWS DataSync</b> khi có đường truyền mạng trực tuyến đủ lớn (Direct Connect/Internet) và cần đồng bộ tự động liên tục. Dùng <b>Snowball Edge</b> khi khối lượng dữ liệu khổng lồ (vài chục đến hàng trăm TB) mà đường truyền mạng on-premise mất nhiều tuần/tháng mới tải xong (Offline transfer).
</details>
