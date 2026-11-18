🚀 BÁO CÁO CÀI ĐẶT & THỰC HÀNH TRUY VẤN HADOOP & HIVE (NHÓM 10)
> **Học phần:** Lưu trữ và Xử lý Dữ liệu lớn (Big Data Storage & Processing)  
> **Nhóm thực hiện:** Nhóm 10  
> **Môi trường triển khai:** Docker Container (Ubuntu 22.04 LTS - Pseudo-Distributed Mode)
---
📌 1. TỔNG QUAN ĐỀ TÀI
Dự án thực hiện xây dựng một môi trường Big Data hoàn chỉnh trên nền tảng Docker, bao gồm việc cài đặt, cấu hình tối ưu cụm Hadoop 3.3.6 (HDFS, YARN MapReduce v2) và Apache Hive 4.0.1 (Metastore Derby Embedded).
Sau khi dựng cụm thành công, dự án tiến hành định nghĩa mô hình dữ liệu cửa hàng linh kiện máy tính (N10Hive), nạp dữ liệu từ Local Windows lên HDFS và thực thi chuỗi truy vấn phức hợp HiveQL (bao gồm dữ liệu cấu trúc, kiểu dữ liệu phức hợp ARRAY/MAP/STRUCT, định dạng ORC tối ưu và thao tác CRUD ACID).
🛠️ Thông số Kỹ thuật Hệ thống:
OS Base Container: Ubuntu 22.04 LTS
Java Runtime: OpenJDK 8 (`java-8-openjdk-amd64`)
Storage Layer: Apache Hadoop HDFS 3.3.6
Compute Layer: Hadoop YARN (MapReduce v2 execution engine)
Data Warehouse: Apache Hive 4.0.1
Metastore Database: Apache Derby 10.14.2.0
Container Name / Host: `hadoop-master`
---
📂 2. CẤU TRÚC KHO TÀI LIỆU & GIAO DIỆN TRỰC QUAN
Repository này tích hợp sẵn bộ giao diện hướng dẫn tương tác trực quan (Interactive Dashboard) bằng HTML/CSS hỗ trợ chế độ Light/Dark Mode:
File / Tài nguyên	Loại	Mô tả nội dung
🌐 `index.html`	Dashboard	Trang chủ điều hành & tổng hợp điều hướng toàn bộ hệ thống
⚙️ `N10HadoopHiveInstall.html`	Guide Trực quan	Hướng dẫn 10 phần cài đặt Docker, Hadoop, Hive & Troubleshooting
🔍 `N10HadoopHiveQuery.html`	Guide Trực quan	Hướng dẫn 10 bước nạp dữ liệu, sửa câu lệnh SQL & 25 bảng HiveQL
📄 Google Docs Report	Báo cáo	Báo cáo chi tiết hoàn chỉnh dạng văn bản
📦 Dataset N10Hive	Dữ liệu gốc	Bộ dữ liệu CSV/TSV phục vụ nạp vào HDFS
🎥 Video Hướng dẫn	Media	Video quay lại toàn bộ quá trình thực hành chi tiết
---
⚡ 3. HƯỚNG DẪN KHỞI CHẠY NHANH (QUICK START)
Bước 1: Khởi tạo Docker Container
Chạy lệnh trên Windows PowerShell:
```powershell
docker run -it -d --name hadoop-master -h hadoop-master `
  -p 10000:10000 -p 10002:10002 -p 9870:9870 -p 8088:8088 -p 19888:19888 `
  ubuntu:22.04
```
Bước 2: Khởi động Cụm dịch vụ
Vào Container và chạy script khởi động tự động:
```bash
docker exec -it hadoop-master bash
/root/start-all.sh
```
Bước 3: Truy cập Giao diện Web Monitoring
HDFS NameNode UI: http://localhost:9870
YARN ResourceManager UI: http://localhost:8088
HiveServer2 Web UI: http://localhost:10002
Hive JDBC Server: `jdbc:hive2://localhost:10000` (User: `root`)
---
📊 4. DANH MỤC TRUY VẤN VÀ CƠ SỞ DỮ LIỆU N10HIVE
Hệ thống quản lý tổng cộng 25 bảng, phân chia thành các nhóm:
15 Bảng gốc: `sp`, `spcuthe`, `khachhang`, `loaisp`, `thieu`, `ctdon`, ...
6 Bảng Big Data mở rộng: `web_events`, `don_morong`, `khachhang_morong`, ...
Các bảng kiểu dữ liệu phức hợp:
`products_array`: Lưu mảng đặc trưng sản phẩm (`ARRAY<STRING>`).
`products_map`: Lưu cặp thông số kỹ thuật (`MAP<STRING, STRING>`).
`products_struct`: Gom nhóm thông tin thương hiệu, bảo hành (`STRUCT<...>`).
`inventory_array`: Thao tác tách mảng chuỗi Serial với `EXPLODE()`.
CRUD & ACID Transactions: Bảng ORC hỗ trợ `INSERT`, `UPDATE`, `DELETE`.
---
🛠️ 5. CÁC LỖI QUAN TRỌNG ĐÃ ĐƯỢC KHẮC PHỤC
Xung đột Protobuf (`NoSuchMethodError ... checkNotNull`): Đã bổ sung cấu hình `mapreduce.job.user.classpath.first=true` và khai báo `hive.aux.jars.path` trỏ đúng bản `protobuf-java-3.24.4.jar` của Hive trong `hive-site.xml`.
Hàm `map_from_entries` không tồn tại trong Hive 4.0.1: Đã chuyển đổi sang phương pháp ghép chuỗi và phân tách bằng `str_to_map()`.
Kẹt YARN Job ở mức 0%: Đã tối ưu `capacity-scheduler.xml` (`maximum-am-resource-percent` = 0.8) và cấu hình lại classpath MapReduce trong `yarn-site.xml`.
---
👥 THÔNG TIN TÁC GIẢ
Thực hiện: Nhóm 10
Học phần: Lưu trữ và Xử lý Dữ liệu lớn
Năm học: 2024 - 2025
Nếu có thắc mắc hoặc cần hỗ trợ về mặt kỹ thuật, vui lòng tham khảo file `N10HadoopHiveInstall.html` hoặc xem file báo cáo đính kèm.
