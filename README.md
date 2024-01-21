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
-	Web browser
-	Cơ sở dữ liệu MongoDB
-	Object Storage MinIO (minio)
-	RabbitMQ

Công cụ hỗ trợ lập trình:
-	Visual Studio Code
-	Google Colaboratory (gọi tắt là Google Colab hay Colab)

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
  <img width="600" src="https://i.imgur.com/XQMUigk.png" alt="Sơ đồ kiến trúc hệ thống nhận diện người lạ trên Nvidia Jetson Nano">
</p>
<p align="center">
  <em>Figure 2: Sơ đồ kiến trúc hệ thống nhận diện người lạ trên Nvidia Jetson Nano</em>
</p>

















# I.	CẤU TRÚC PHẦN MỀM
# II. CÁC THÀNH PHẦN CỦA HỆ THỐNG
# III.	TÍNH NĂNG HIGH AVAILABILITY, SCALABILITY, RELIABILITY CHO HỆ THỐNG
# IV.	TRIỂN KHAI HỆ THỐNG
