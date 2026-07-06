---
title: "Worklog Tuần 8"
date: 2026-05-31
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Mục tiêu tuần 8:
* Làm quen với Data Engineering trên AWS: Glue, Athena, QuickSight.
* Xử lý dữ liệu real-time với Amazon Kinesis và MSK (Kafka).
* Xây dựng pipeline dữ liệu từ ingestion → processing → analytics → visualization.
* Thực hành các tình huống phân tích dữ liệu thực tế và tối ưu chi phí truy vấn.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                            | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tổng quan Data Engineering trên AWS: <br>&emsp; + Data Lake vs Data Warehouse vs Data Lakehouse <br>&emsp; + Batch processing vs Stream processing <br>&emsp; + Kiến trúc Lambda vs Kappa <br> - Tìm hiểu AWS Glue: <br>&emsp; + Data Catalog <br>&emsp; + Crawler (tự sinh schema) <br>&emsp; + ETL Job (PySpark) <br>&emsp; + Glue Studio (visual ETL) <br>&emsp; + Glue DataBrew (no-code data prep)                                                                                          | 01/06/2026   | 01/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu Amazon Athena: <br>&emsp; + Serverless query engine trên S3 <br>&emsp; + Các định dạng: Parquet, ORC, Avro, JSON, CSV <br>&emsp; + Partitioning & Compression <br>&emsp; + Federated Query (RDS, DynamoDB) <br>&emsp; + Query Result Location <br> - **Thực hành:** Tạo Data Catalog cho file CSV trên S3, viết query phân tích doanh thu theo tháng, top 10 sản phẩm bán chạy                                                                                                                                                                                  | 02/06/2026   | 02/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu Amazon Kinesis: <br>&emsp; + Kinesis Data Streams (low-level, custom consumer) <br>&emsp; + Kinesis Data Firehose (managed delivery → S3/Redshift/OpenSearch) <br>&emsp; + Kinesis Data Analytics (SQL tumbling window) <br> - Tìm hiểu Amazon MSK (Managed Streaming for Apache Kafka): <br>&emsp; + Cluster, broker, topic <br>&emsp; + Multi-AZ replication <br> - **Thực hành:** Mô phỏng log IoT stream → Firehose → S3 (Parquet) → Athena                                                                                                                       | 03/06/2026   | 03/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu Amazon QuickSight: <br>&emsp; + Dataset, Analysis, Dashboard <br>&emsp; + Kết nối nguồn: Athena, RDS, S3, Redshift <br>&emsp; + Calculated field, filter, parameter, KPI <br>&emsp; + ML Insights (anomaly detection, forecasting) <br>&emsp; + Embedded Analytics <br> - **Thực hành:** Tạo dashboard phân tích doanh thu theo khu vực, thời gian, nhóm sản phẩm; chia sẻ cho team                                                                                                                                                                       | 04/06/2026   | 05/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Lab tổng hợp tuần 8:** <br>&emsp; + Pipeline: Kinesis Firehose → S3 (raw JSON) → Glue ETL (clean) → S3 (Parquet partition) → Athena → QuickSight <br>&emsp; + Dữ liệu mẫu: log bán hàng thời gian thực <br>&emsp; + Tổng kết & cập nhật worklog, viết blog chia sẻ pipeline dữ liệu trên AWS                                                                                                                                                                                                          | 05/06/2026   | 05/06/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 8:
* Nắm vững các khái niệm Data Engineering:
  * Phân biệt Data Lake (S3), Data Warehouse (Redshift), Data Lakehouse (S3 + Delta Lake)
  * Hiểu batch processing (Glue, EMR) vs stream processing (Kinesis, MSK)
  * So sánh kiến trúc Lambda (batch + speed layer) vs Kappa (chỉ stream)

* Làm chủ AWS Glue:
  * Dùng Crawler tự động sinh schema cho dữ liệu trên S3
  * Viết ETL Job bằng PySpark chuẩn hoá, làm sạch dữ liệu (drop null, cast type, rename column)
  * Lưu trữ metadata vào Glue Data Catalog (có thể query từ Athena, Redshift Spectrum)
  * Glue Studio cho phép thiết kế ETL kéo thả trực quan
  * Glue DataBrew giúp analyst chuẩn bị dữ liệu không cần code

* Truy vấn dữ liệu bằng Athena:
  * Chạy SQL trên file Parquet/CSV ở S3 mà không cần ETL
  * Partition (year/month/day) & chuyển đổi sang Parquet giúp giảm chi phí 70-90%
  * Dùng CTAS (Create Table As Select) để tạo bảng tổng hợp trước
  * Sử dụng Athena Workgroup để tách chi phí giữa các team
  * Federated Query: kết nối trực tiếp tới RDS PostgreSQL/MySQL

* Xử lý dữ liệu stream với Kinesis:
  * **Kinesis Data Streams:** tiêu thụ real-time, custom consumer (Lambda, KCL)
  * **Kinesis Data Firehose:** tự động buffer (60s hoặc 128MB) rồi ghi xuống S3/Redshift/OpenSearch
  * **Kinesis Data Analytics:** chạy SQL tumbling window trên stream
  * So sánh với MSK: chọn Kinesis khi cần fully-managed; chọn MSK khi đã quen Kafka

* Trực quan hoá dữ liệu bằng QuickSight:
  * Datasets từ Athena (datasets quan hệ, datasets SPICE in-memory)
  * Dashboard phân tích doanh thu, top sản phẩm, churn rate
  * Calculated field, filter, parameter kéo thả dễ dàng
  * ML Insights phát hiện bất thường & dự báo chuỗi thời gian
  * Embedded Analytics nhúng dashboard vào web app khác

* Hoàn thành pipeline dữ liệu hoàn chỉnh:
  * **Ingestion:** Kinesis Firehose thu thập log real-time
  * **Storage:** S3 lưu raw JSON (partition theo year/month/day)
  * **Processing:** Glue ETL làm sạch, chuyển sang Parquet
  * **Querying:** Athena truy vấn SQL không cần server
  * **Visualization:** QuickSight dashboard chia sẻ cho team

* Viết blog chia sẻ pipeline dữ liệu trên AWS giúp cộng đồng học hỏi.
* Thói quen mới:
  * Mỗi dataset trên S3 đều partition theo date để tối ưu chi phí query
  * Glue Job chạy theo schedule (cron) qua EventBridge
  * Tag dataset theo `DataClass = Confidential/Public` để quản lý truy cập
  * Theo dõi chi phí Athena qua Cost Explorer & set budget riêng
* ...