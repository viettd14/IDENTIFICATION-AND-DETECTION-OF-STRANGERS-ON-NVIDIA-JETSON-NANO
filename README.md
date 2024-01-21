<div align="center">
  <h1><strong>INTEGRATING ALGORITHMS FOR RECOGNITION AND DETECTION OF STRANGERS ON NVIDIA JETSON NANO</strong></h1>
</div>

### Abstract
NVIDIA Jetson Nano embedded computers are being used in research to integrate face recognition and stranger detection algorithms in Smart Homes.

### Keywords
NVIDIA Jetson Nano, SmartHome, face recognition, OpenCV with CUDA for Jetson Nano.

# I. INTRODUCTION
<p align="justify"> The face recognition by using smart camera with Smart Home system would bring some benefits such as: face recognition for opening doors, internal light control combining with face recognition, or the security system of Smart Home would notify the stranger in house by comparing with the stored family’s members in the device. Besides, system could store video for easily review and snapshot for investigation. </p>

## 1.1.	Giới thiệu bài toán
Hệ thống camera quan sát thông minh nhận diện khuôn mặt phối hợp cùng nhà thông minh Smart Home sẽ mang đến những lợi ích như:
-	Nhận diện khuôn mặt để mở cửa.
-	Nhận diện khuôn mặt, chuyển động để bật tắt đèn trong phòng một cách tự động, thông minh.
-	Phối hợp với hệ thống an ninh của nhà thông minh Smart Home giúp thông báo sự xuất hiện của chuyển động lạ hoặc những khuôn mặt người lạ so với khuôn mặt trong danh sách mà chủ nhà đã cài đặt. 
-	Lưu trữ video thông minh: Hệ thống lưu trữ sẽ phân biệt thời gian nào là khoảng thời gian tĩnh, thời gian nào là có chuyển động, và thời gian nào camera phát hiện ra gương mặt. Qua đó giúp bạn dễ dàng trích xuất hoặc xem lại sự kiện đã diễn ra.

### a.	Mục tiêu tổng quan: 
Nghiên cứu tích hợp thuật toán nhận diện và phát hiện người lạ trong nhà thông minh (SmartHome) trên máy tính nhúng NVIDIA Jetson Nano

### b.	Mục tiêu cụ thể:
-	Thuật toán nhận diện người lạ trên thiết bị nhúng
-	Hiện thực thuật toán nhận diện người lạ trên NVIDIA Jetson Nano
-	Thực nghiệm và đánh giá kết quả
  
#### c.	Phương pháp thực hiện
`Thiết bị phần cứng:`
-	NVIDIA Jetson Nano
-	Camera để chụp hình và nhận diện
-	Micro SD để lưu trữ
-	Keyboard + Mouse để thao tác
-	Monitor + cáp HDMI để hiển thị
-	Case và quạt tản nhiệt cho Jetson Nano.

`Thư viện, môi trường hỗ trợ lập trình ứng dụng:`  
Sử dụng công nghệ:
-	Python
-	ReactJS
-	NodeJS
-	Material UI
-	MongoDB
-	Object Storage MinIO (minio)
-	RabbitMQ

Công cụ hỗ trợ lập trình:
-	Visual Studio Code
-	Google Colaboratory

Thư viện sử dụng:
-	OpenCV (Open Computer Vision)
-	Haar Cascade

## 1.2.	Ý nghĩa bài toán
<p align="justify"> Phát hiện và nhận diện khuôn mặt người cũng là một chủ đề rất quan trọng đối với các hoạt động theo dõi và bảo mật. Mặc dù các công cụ và thuật toán được sử dụng trong thư viện này không mạnh mẽ và an toàn như các hệ thống thương mại, nhưng nó có rất nhiều tiềm năng để thay thế với một vài chỉnh sửa về thiết kế và triển khai.</p>

## 1.3.	Giới hạn và kết quả mong muốn
<p align="justify"> Dựa trên tập dữ liệu mẫu về khuôn mặt các thành viên trong nhóm, hệ thống sẽ được training để xác định khuôn mặt người lạ (không phải là thành viên nhóm) khi hình ảnh được chụp từ camera kết nối trực tiếp.</p>



# II. THIẾT KẾ HỆ THỐNG
## 2.1 Kiến trúc hệ thống
`FRONT-END:`
<p align="center">
  <img width="450" src="https://i.imgur.com/XQMUigk.png" alt="Sơ đồ kiến trúc hệ thống nhận diện người lạ trên Nvidia Jetson Nano">
</p>
<p align="center">
  <em>Figure 1: Sơ đồ kiến trúc hệ thống nhận diện người lạ trên Nvidia Jetson Nano</em>
</p>

`BACK-END:`
<p align="center">
  <img width="600" src="https://i.imgur.com/xs9WoEH.png" alt="Sơ đồ kiến trúc hệ thống back-end">
</p>
<p align="center">
  <em>Figure 2: Sơ đồ kiến trúc hệ thống back-end</em>
</p>

Hệ thống phân thành các tầng xử lý nhằm tăng tính linh hoạt cho hệ thống:
- Lớp giao tiếp với người dùng hiển thị giao tiếp với người dùng (UI) hoặc giao tiếp với các hệ thống khác (API/SDK). Đây là phần ứng dụng ở phía người dùng cuối. Người dùng sẽ sử dụng những ứng dụng này để truy xuất vào các chức năng của ứng dụng.
- Lớp xử lý nghiệp vụ:
  -	API Gateway: cổng giao tiếp từ lớp giao tiếp đến các service bên trong xử lý nghiệp vụ. API Gateway sẽ nhận các yêu cầu từ phía client, chỉnh sửa, xác thực và điều hướng chung đến các API cụ thể trên services phía sau.
  -	Service Hệ thống nhúng:
    -	Quản lý thông tin người lạ: Service xử lý các thông tin về người lạ.
    -	Cảnh báo người lạ: Service xử lý thời gian thực cảnh báo phía client khi nhận được message từ Message Broker.
    -	RabbitMQ: cung cấp giao thức truyền thông nội bộ giữa các service.
  - Lớp dữ liệu: nơi đây cung cấp các chức năng để quản trị và khai thác dữ liệu. Các dữ liệu chính bao gồm: NoSQL (MongoDB), Object Storage (MinIO).
  -	Lớp cơ sở hạ tầng: hạ tầng ảo hóa phục vụ hệ thống.

## 2.2.	Các thành phần của hệ thống
### 2.2.1.	Web Application
-	Được build dưới dạng Image thông qua cấu hình được đọc từ Dockerfile và upload lên Docker Hub.
-	Web Application cũng là nơi cung cấp giao diện để tương tác với người dùng.
	
### 2.2.2.	Database
-	Database được cài đặt trực tiếp lên máy chủ với đầy đủ phân quyền và bảo mật đối với mỗi đối tượng.
-	Database cho service: MongoDB.
-	Database cho file/object: MinIO.
  
###	2.2.3. Docker
-	Cung cấp những công cụ và service để đóng gói và chạy chương trình của mình trên các môi trường khác nhau một cách nhanh nhất.
-	Image: là file ảnh, file nền của một hệ điều hành, một nền tảng, một ngôn ngữ (ví dụ: Ubuntu Image, Ruby Image, Rails Image, MySQL Image, …). Các Image này sẽ dùng để tạo ra các Container.
-	Container: là một máy ảo, được cấu thành từ 1 Image và 1 lớp decoration writable-file-layer. Các thay đổi trong máy ảo (cài thêm phần mềm, tạo thêm file, …) sẽ được lưu ở lớp decoration này.
-	Các Container này sẽ dùng chung tài nguyên của hệ thống (RAM, Disk, Network, …), chính nhờ vậy, những Container sẽ rất nhẹ, việc khởi động, tương tác sẽ rất nhanh gọn.
-	Docker Engine: quản lý việc tạo Image, chạy Container, dùng Image có sẵn hay tải Image chưa có về, kết nối vào Container, thêm/ sửa/ xóa Image và Container,…
-	Docker Registry: Kho lữu trữ các Image.

### 2.2.4. Kubernetes
-	Hệ thống Kubernetes được sử dụng trong Hệ thống Cảnh báo người lạ được thiết lập vừa là Master Node vừa là Worker Node với số lượng Replica Set là 1.
-	Kubernetes Cluster: là một tập các máy vật lý hay máy ảo được sử dụng bởi Kubernetes để chạy các ứng dụng. Kubernetes Cluster bao gồm Node (Master) và Node (Worker).
-	Nodes: một node là một máy ảo hoặc máy vật lý chạy Kubernetes.
-	Pods: Pod là 1 nhóm (1 trở lên) các Container thực hiện một mục đích nào đó, như là chạy Software nào đó. Nhóm này chia sẻ không gian lưu trữ, địa chỉ IP với nhau. Pod được tạo ra hoặc xóa tùy thuộc vào yêu càu của dự án. Đơn vị nhỏ nhất của ứng dụng chạy Kubernetes đó là Container, nhưng đơn vị quản lý cơ bản nhất thì là Pods.
-	Deployment: dùng để phân phối các Pods cho các Nodes cụ thể, mang tính dự phòng.
-	Replica Set: đảm nhận vai trò tạo ra số lượng Pods giống như dựa vào yêu cầu và luôn duy trì số lượng đó.

Các chức năng của Kubernetes:
-	Docker Hosts Management.
-	Container Scheduling.
-	Rolling Update.
-	Scaling/Auto Scaling.
- Monitor the life cycle and status of Containers.
-	Self-Hearing.
-	Service Discovery.
-	Load Balancing.
-	Data Management, Logs Management
- Worker Node Management.
-	Infrastructure as Code.









# I.	CẤU TRÚC PHẦN MỀM
# II. CÁC THÀNH PHẦN CỦA HỆ THỐNG
# III.	TÍNH NĂNG HIGH AVAILABILITY, SCALABILITY, RELIABILITY CHO HỆ THỐNG
# IV.	TRIỂN KHAI HỆ THỐNG

<p align="justify"></p> 
