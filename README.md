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
- Nhận diện khuôn mặt để mở cửa.
- Nhận diện khuôn mặt, chuyển động để bật tắt đèn trong phòng một cách tự động, thông minh.
- Phối hợp với hệ thống an ninh của nhà thông minh Smart Home giúp thông báo sự xuất hiện của chuyển động lạ hoặc những khuôn mặt người lạ so với khuôn mặt trong danh sách mà chủ nhà đã cài đặt. 
- Lưu trữ video thông minh: Hệ thống lưu trữ sẽ phân biệt thời gian nào là khoảng thời gian tĩnh, thời gian nào là có chuyển động, và thời gian nào camera phát hiện ra gương mặt. Qua đó giúp bạn dễ dàng trích xuất hoặc xem lại sự kiện đã diễn ra.

### a.	Mục tiêu tổng quan: 
Nghiên cứu tích hợp thuật toán nhận diện và phát hiện người lạ trong nhà thông minh (SmartHome) trên máy tính nhúng NVIDIA Jetson Nano

### b.	Mục tiêu cụ thể:
- Thuật toán nhận diện người lạ trên thiết bị nhúng
- Hiện thực thuật toán nhận diện người lạ trên NVIDIA Jetson Nano
- Thực nghiệm và đánh giá kết quả
  
#### c.	Phương pháp thực hiện
`Thiết bị phần cứng:`
- NVIDIA Jetson Nano
- Camera, Micro SD.
- Keyboard + Mouse.
- Monitor + HDMI Cable.
- Case + Cooler Fan.

`Thư viện, môi trường hỗ trợ lập trình ứng dụng:`  
Sử dụng công nghệ:
- Python, ReactJS, NodeJS.
- Material UI
- MongoDB, RabbitMQ.
- Object Storage MinIO (minio)

Công cụ hỗ trợ lập trình:
- Visual Studio Code
- Google Colaboratory

Thư viện sử dụng:
- OpenCV (Open Computer Vision)
- Haar Cascade

## 1.2.	Ý nghĩa bài toán
<p align="justify"> Phát hiện và nhận diện khuôn mặt người cũng là một chủ đề rất quan trọng đối với các hoạt động theo dõi và bảo mật. Mặc dù các công cụ và thuật toán được sử dụng trong thư viện này không mạnh mẽ và an toàn như các hệ thống thương mại, nhưng nó có rất nhiều tiềm năng để thay thế với một vài chỉnh sửa về thiết kế và triển khai.</p>

## 1.3.	Giới hạn và kết quả mong muốn
<p align="justify"> Dựa trên tập dữ liệu mẫu về khuôn mặt các thành viên trong nhóm nghiên cứu, hệ thống sẽ được training để xác định khuôn mặt người lạ (không phải là thành viên nhóm) khi hình ảnh được chụp từ camera kết nối trực tiếp.</p>



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
  - API Gateway: cổng giao tiếp từ lớp giao tiếp đến các service bên trong xử lý nghiệp vụ. API Gateway sẽ nhận các yêu cầu từ phía client, chỉnh sửa, xác thực và điều hướng chung đến các API cụ thể trên services phía sau.
  - Service Hệ thống nhúng:
    - Quản lý thông tin người lạ: Service xử lý các thông tin về người lạ.
    - Cảnh báo người lạ: Service xử lý thời gian thực cảnh báo phía client khi nhận được message từ Message Broker.
    - RabbitMQ: cung cấp giao thức truyền thông nội bộ giữa các service.
  - Lớp dữ liệu: nơi đây cung cấp các chức năng để quản trị và khai thác dữ liệu. Các dữ liệu chính bao gồm: NoSQL (MongoDB), Object Storage (MinIO).
  - Lớp cơ sở hạ tầng: hạ tầng ảo hóa phục vụ hệ thống.

## 2.2.	Các thành phần của hệ thống
### 2.2.1. Web Application
- Được build dưới dạng Image thông qua cấu hình được đọc từ Dockerfile và upload lên Docker Hub.
- Web Application cũng là nơi cung cấp giao diện để tương tác với người dùng.
	
### 2.2.2. Database
- Database được cài đặt trực tiếp lên máy chủ với đầy đủ phân quyền và bảo mật đối với mỗi đối tượng.
- Database cho service: MongoDB.
- Database cho file/object: MinIO.
  
### 2.2.3. Docker
- Cung cấp những công cụ và service để đóng gói và chạy project trên các môi trường khác nhau một cách nhanh nhất.

### 2.2.4. Kubernetes
- Hệ thống Kubernetes được sử dụng trong Hệ thống Cảnh báo người lạ được thiết lập vừa là Master Node vừa là Worker Node với số lượng Replica Set là 1.

Các chức năng của Kubernetes:
- Management: Docker Hosts, Rolling Update, Logs, Data, Worker Node.
- Monitor: Container Scheduling, The life cycle and Status of Containers.
- Scaling/Auto Scaling.
- Self-Hearing.
- Service Discovery.
- Load Balancing.
- Infrastructure as Code.

### 2.2.5. Rancher
### 2.2.6. High Availability, Scalability, Reliability cho các Pod/Service trong Cluster
<p align="justify"> Bằng việc sử dụng kết hợp 3 thành phần: Docker, Kubernetes và Rancher, toàn bộ các thành phần được triển khai trên nền tảng Cloud Platform với 3 thành phần trên đều được đảm bảo tính High Availability, Scability, Reliability cho các ứng dụng.</p> 


## 2.3. Kiến trúc thuật toán nhận diện khuôn mặt
Dưới đây là sơ đồ mô tả luồng hoạt động chính của thuật toán:
 <p align="center">
  <img width="450" src="https://i.imgur.com/Q4VnyHF.png" alt="Sơ đồ luồng hoạt động chính của thuật toán nhận diện">
</p>
<p align="center">
  <em>Figure 3: Sơ đồ luồng hoạt động chính của thuật toán nhận diện</em>
</p>


## 2.4. Thiết lập phần cứng 
Giao diện phần cứng bao gồm các bước sau:
- Tải và cài đặt file image cho Jetson Nano từ trang chủ của Ndivia
- Kết nối Mô-đun máy ảnh Raspberry Pi V2 với Nvidia Jetson Nano bằng cáp Raspberry cho camera
- Kết nối bảng hiển thị Màn hình cảm ứng với Nvidia Jetson Nano bằng một cáp HDMI
- Cấp nguồn cho bo mạch Nvidia Jetson Nano bằng bộ chuyển đổi nguồn AC-DC cung cấp nguồn 5V
- Kết nối chuột, bàn phím không dây cũng như wifi USB cho Jetson Nano.
<p align="center">
  <img width="450" src="https://i.imgur.com/SoNGHnD.png" alt="jetson nano system hardware">
</p>
<p align="center">
  <em>Figure 4: Jetson Nano system hardware</em>
</p>


## 2.5. Thiết lập phần mềm
### 2.5.1. FRONT-END
<p align="center">
  <img width="500" src="https://i.imgur.com/EiujrP8.png" alt="Kiến trúc lớp cho dự án nhận diện người lạ trong ứng dụng SmartHome">
</p>
<p align="center">
  <em>Figure 5: Kiến trúc lớp cho dự án nhận diện người lạ trong ứng dụng SmartHome</em>
</p>

Để định cấu hình Nvidia Jetson Nano với hệ điều hành Ubuntu và cài đặt các gói thư viện thích hợp, các thư viện quan trọng nhất được sử dụng trong dự án là:
- `dlib:` Một thư viện deep learning được viết bằng C ++ với các ràng buộc cho Python.
- `face_recognition:` Một gói Python với phần mềm phụ trợ dlib chạy thư viện nhận diện khuôn mặt hiện đại của dlib.
- `Gstreamer:`một gói trong Python cho phép tương tác với cái camera cũng như xử lý tín hiệu âm thanh hình ảnh từ camera trong code Python.
- `Pi.GPIO:` Thư viện để điều khiển đầu ra chân GPIO của Nvidia Jetson Nano.
- `PyQt5:` Liên kết Python cho thư viện Qt C ++ để phát triển giao diện người dùng và công cụ hiển thị.
- `OpenCV:` là một thư viện mã nguồn mở hàng đầu cho thị giác máy tính (computer vision), xử lý ảnh và deep learning, và các tính năng tăng tốc GPU trong hoạt động thời gian thực, bao gồm các interface C++, C, Python, Java và hỗ trợ Windows, Linux, Mac OS, iOS và Android. OpenCV được thiết kế để tính toán hiệu quả và với sự tập trung nhiều vào các ứng dụng thời gian thực.
- `Requests:` thư viện hỗ trợ việc gọi và xử lý cái api thông qua network trong Python.

### 2.5.2. BACK-END
<p align="center">
  <img width="500" src="https://i.imgur.com/wBMRzwF.png" alt="Mô hình tổng quan (Low Level Design) hệ thống back-end">
</p>
<p align="center">
  <em>Figure 6: Mô hình tổng quan (Low Level Design) hệ thống back-end</em>
</p>

Hệ thống máy chủ gồm 2 máy chủ ảo hóa:
- Server 1:
  - 4 CPU – 4 GB RAM – 80 GB Disk
  - 1 IP Public: 171.244.38.5
- Server 2:
  - 4 CPU – 4 GB RAM – 80 GB Disk
  - 1 IP Public: 171.244.38.7
- Các kết nối:
  - Mỗi máy chủ ảo hóa đều được cấp 1 IP Public và mở các Port cho các Service giao tiếp ra bên ngoài đồng thời giao tiếp nội bộ giữa các máy chủ.
  - Cluster sẽ được Load Balance thông qua phần mềm HAProxy & Keepalived.

<p align="center">
  <img width="500" src="https://i.imgur.com/0oZINPO.png" alt="Mô hình triển khai (High Level Design) hệ thống back-end">
</p>
<p align="center">
  <em>Figure 7: Mô hình triển khai (High Level Design) hệ thống back-end</em>
</p>

- Cài đặt triển khai mô hình Rancher/Kubernetes (Master Nodes và Worker Nodes)
  - Máy chủ Kubernetes vừa đóng vai trò là Master vừa là Worker.
  - Triển khai các service: Web Application Service và API Service.
  - Các Worker Nodes có thể giao tiếp trực tiếp với nhau thông qua IP/Hostname hoặc giao tiếp với nhau thông qua RabbitMQ.
  - Các Worker Nodes được quản lý và điều phối thông qua Master Nodes.
  - Các Worker Nodes được cấu hình Health Check để đảm bảo tính HA.
  - Hệ thống giao tiếp với bên ngoài thông qua Load Balancer và Firewall.
- Database:
  - Cài đặt MongoDB trực tiếp trên máy chủ với cấu hình Single Node.
  - Dữ liệu trong Database được lưu trữ trên Host và có cơ chế backup & restore tự động.
- Các Service:
  - Web Application Service: sử dụng thư viện Javascript-front-end ReactJS để xây dựng giao diện người dùng.
  - API Service: sử dụng Framework NodeJS với ngôn ngữ lập trình TypeScript.
  - Cả 2 Service đều được build thành các Image tương ứng với cấu hình lệnh trong Dockerfile và tiến hành Deploy lên Docker Hub, sau đó hệ thống Kubernetes (K3s) sẽ Pull Image tương ứng với từng cấu hình với đuôi file .YAML và tự động Deploy theo Port đã thiết lập với trạng thái môi trường Production.

- Thiết lập website cảnh báo người lạ:
Link website: http://my-server:30003/

<p align="center">
  <img width="500" src="https://i.imgur.com/H9FEWue.png" alt="Giao diện website cảnh báo người lạ">
</p>
<p align="center">
  <em>Figure 8: Giao diện website cảnh báo người lạ</em>
</p>




# III.	TRIỂN KHAI HỆ THỐNG

<p align="justify"></p> 
