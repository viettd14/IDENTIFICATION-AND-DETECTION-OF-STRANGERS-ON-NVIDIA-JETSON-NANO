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



# III. TRIỂN KHAI HỆ THỐNG
## 3.1. Kiến trúc thuật toán nhận diện khuôn mặt
Dưới đây là sơ đồ mô tả luồng hoạt động chính của thuật toán:
 <p align="center">
  <img width="450" src="https://i.imgur.com/Q4VnyHF.png" alt="Sơ đồ luồng hoạt động chính của thuật toán nhận diện">
</p>
<p align="center">
  <em>Figure 3: Sơ đồ luồng hoạt động chính của thuật toán nhận diện</em>
</p>


## 3.2. Triển khai phần cứng 
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


## 3.2. Triển khai phần mềm
### 3.2.1. FRONT-END
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

### 3.2.2. BACK-END
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



# IV. ĐÁNH GIÁ GIẢI PHÁP
## 4.1. Mô hình kiểm tra 
- Kiểm tra độ chính xác của gương mặt các thành viên trong nhóm sau khi đã training cho máy học để có thể nhận diện và phân biệt với người lạ.
- Hệ thống kiểm tra trực tiếp hình ảnh người lạ thông qua camera hoặc là hình ảnh có trong điện thoai hoặc in ra giấy.
  
## 4.2. Mô hình đánh giá
Tiến hành đo đạc kết quả train 6 gương mặt các thành viên trong nhóm với tập dataset hình ảnh là 180 hình:
<p align="center">
  <img width="500" src="https://i.imgur.com/Vbc1ZA3.png"
</p>

Tiết quả train 6 gương mặt các thành viên trong nhóm với tập dataset hình ảnh là 3000 hình:
<p align="center">
  <img width="500" src="https://i.imgur.com/leOsrow.png"
</p>
	
- Với việc hình gương mặt đã được scale lại cũng như loại bỏ màu sắc để chuyển về dạng xám, tốc độ training cho các tập dữ liệu mẫu trên thiết bị Jetson Nano đạt được lần lượt là 3,35 giây cho 180 hình và 15,6 giây cho 3000 với thuận toán nhận diện Haar Cascade.

Tiến hành đánh giá độ chính xác của hệ thống thông qua việc nhận diện được các thành viên trong nhóm:
- Trong điều kiện đầy đủ ánh sáng, góc chính diện, kết quả nhận diện đo được khoảng hơn 80% với hình ảnh trực tiếp từ camera và hơn 60% với hình ảnh thông qua các thiết bị di động hoặc in giấy.
- Trong điều kiện thiếu ánh sáng, góc chính diện, kết quả nhận diện đo được khoảng hơn 70% với hình ảnh trực tiếp từ camera và hơn 50% với hình ảnh thông qua các thiết bị di động hoặc in giấy.
- Trong điều kiện các góc nghiêng hoặc bị che, kết quả nhận diện đo được khoảng dưới 60% với hình ảnh trực tiếp từ camera và không nhận diện được hình ảnh thông qua các thiết bị di động hoặc in giấy.
- Trong điều kiện đeo khẩu trang và lộ hơn một nửa khuôn mặt, kết quả nhận diện đo được ở mức trên 70%. Nếu đeo khẩu trang che quá nửa khuôn mặt, hệ thống sẽ không nhận diện được.

Thực hiện đánh giá các thông số của phần cứng Jetson Nano trong quá trình nhận diện khuôn mặt với kết quả đo như sau:
<p align="center">
  <img width="500" src="https://i.imgur.com/ZLBdA6A.png"
</p>
- Khi hệ thống Jetson Nano khởi động camera và thực hiện việc nhận diện khuôn mặt thông qua hình ảnh truyền trực tiếp, lượng tài nguyên CPU của thiết bị được sử dụng đo được ở mức trên 70% của các core và chiếm khoảng 2,4 GB RAM.
Thực hiện kiểm tra khả năng phát hiện và nhận diện người lạ, đồng thời truyền tải tín hiệu về cho server để cập nhật liên tục và hiển thị kịp thời cho người sử dụng.
	
 ## 4.3. Kết quả
Thuật toán Haar Cascade cho kết quả rất nhanh khi phân tích thời gian thực, độ chính xác khi nhận diện ở mức tốt, thời gian training dữ liệu ở khá nhanh là những tiêu chí phù hợp với các thiết bị nhúng như Jetson Nano. 
Tuy nhiên, việc phụ thuộc nhiều vào tập dữ liệu train (càng nhiều dữ liệu thì chi tiết càng cao) cũng như là việc chỉ nhận diện được toàn bộ khuôn mặt ở góc chính diện cũng như là phụ thuộc vào điều kiện ánh sáng môi trường xung quanh là những điểm hạn chế khi cài đặt thuật toán Haar Cascade lên Jetson Nano.

User
| Test case | Training | Conditions | With full light | Without full light |
|:---------:|:--------:|:----------:|:---------------:|:------------------:|
| 30 image/ person | 03.353950 (seconds) | Normal | 72% | - |
|  |  | With mask | - | - |
|  |  | Image in mobile | 50% | - |
| 30 image/ person | 03.353950 (seconds) | Normal | 86% | 70% |
|  |  | With mask | 75% | - |
|  |  | Image in mobile | 60% | 50% |

# Tài liệu tham khảo
1. CPU Performance Comparison of OpenCV and other Deep Learning frameworks, Available: https://learnopencv.com/cpu-performance-comparison-of-opencv-and-other-deep-learning-frameworks/   
2. Brief history of face recognition software, Available:  https://www.facefirst.com/blog/brief-history-of-face-recognition-software/ 
3. Facial recognition: Top 7 trends (tech, vendors, markets, use cases & latest news), Available: https://www.thalesgroup.com/en/markets/digital-identity-and-security/government/biometrics/facial-recognition  
4. OpenCV "DNN" with NVIDIA GPUs: 1549% faster YOLO, SSD, and Mask R-CNN, Available: https://www.pyimagesearch.com/2020/02/10/opencv-dnn-with-nvidia-gpus-1549-faster-yolo-ssd-and-mask-r-cnn/ 
5. Getting Started with Jetson Nano Developer Kit, Available: https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#intro 
6. Face recognition with OpenCV, Python, and deep learning, Available: https://www.pyimagesearch.com/2018/06/18/face-recognition-with-opencv-Python-and-deep-learning/
7. Jetson Community Projects, Available:  https://developer.nvidia.com/embedded/community/jetson-projects
8. Raspberry Pi Face Recognition-Based Door Lock, Available: https://www.digikey.be/en/maker/projects/raspberry-pi-face-recognition-based-door-lock/a4c71ade7f294a62bd8cd94a803df919 
9. Install Opencv 4.1 on Nvidia Jetson Nano, Available: https://pysource.com/2019/08/26/install-opencv-4-1-on-nvidia-jetson-nano/ 
10. How to Install OpenCV 4.5 on NVIDIA Jetson Nano, Available: https://automaticaddison.com/how-to-install-opencv-4-5-on-nvidia-jetson-nano/  
11. Setting up a High-availability K3s Kubernetes Cluster for Rancher, Available: https://rancher.com/docs/rancher/v2.x/en/installation/resources/k8s-tutorials/ha-with-external-db/  
12. Installing Helm, Available: https://helm.sh/docs/intro/install/#helm  
13. Install/Upgrade Rancher on a Kubernetes Cluster, Available: https://rancher.com/docs/rancher/v2.x/en/installation/install-rancher-on-k8s/    
14. Getting started with the Kubernetes Ingress Controller, Available: https://docs.konghq.com/kubernetes-ingress-controller/1.2.x/guides/getting-started/ 
