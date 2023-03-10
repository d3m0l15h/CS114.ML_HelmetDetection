# CS114_ML
| MSSV | Họ và tên |
|---|---|
| 20521102 | Lương Thái Bảo |
| 20521481  | Phan Minh Khôi |
| 21521930 | Đào Tiến Đạt |
---
# **Đề tài: Nhận diện người đội và không đội mũ bảo hiểm khi tham gia giao thông từ hình ảnh hoặc video.**
***1. Mô tả bài toán:***
+ **Input**: Một ảnh màu có một hoặc nhiều đối tượng đội nón bảo hiểm hoặc đối tượng không đội nón bảo hiểm.
+ **Output**: Một hoặc nhiều bounding box (nhãn + vị trí) của đối tượng đổi nón bảo hiểm hoặc đối tượng không đội nón bảo hiểm.
+ **Ngữ cảnh ứng dụng**: Trong đời sống hằng ngày, khi tham gia giao thông luôn tiềm tàng những nguy hiểm nhưng nhờ có nón bảo hiểm đã giảm đáng kể những vụ tử vong đáng tiếc xảy ra. Tuy nhiên vẫn còn nhiều người thờ ơ với tính mạng của bản thân không chấp hành đúng luật giao thông.Nhiều người có thói quen chấp hành luật giao thông khi ở nơi có cảnh sát giao thông nhưng số lượng cảnh sát có hạn nên không thể trực mọi nơi được. Chính vì vậy với sự phát triển của máy móc sẽ hỗ trợ con người trong việc phát hiện những người đội nón bảo hiểm từ đó mở rộng quy mô đến nhiều lớp hơn trong đó có những người không đội nón bảo hiểm để xử phạt họ.


***2. Mô tả về bộ dữ liệu:***
+ Cách thức xây dựng bộ dữ liệu:
  + Tự xây dựng dữ liệu với hình ảnh được lấy từ camera giao thông của [Sở giao thông vận tải](http://giaothong.hochiminhcity.gov.vn/map.aspx) và [Insecam](http://www.insecam.org/en/bycountry/VN/).
  + Ảnh lấy được từ data:
![example](/images/img_example.jpeg)
+ Số lượng dữ liệu: **1109 ảnh**
+ Các thao tác tiền xử lý dữ liệu: 
  + Resize ảnh về kích thước: 640x640 pixel 
  + Sử dụng [Roboflow](https://roboflow.com) để tạo label cho các đối tượng trong ảnh (đối tượng ở đây là đầu của người tham gia giao thông). Ứng với mỗi ảnh là một file `.txt` chứa thông tin của label phù hợp với format của YOLOv5.
  + Nội dung file `.txt` bao gồm: `class_id` `center_x` `center_y` `width` `height`
    + Ví dụ: `1 0.37109375 0.51171875 0.021875 0.03515625`
+ Phân chia (split) - train/dev/test:
  + Training set: **800 ảnh**
  + Validation set: **175 ảnh**
  + Testing set: **134 ảnh**
+ Số lượng label:
  + With helmet: 4145 đối tượng
  + Without helmet: 660 đối tượng

***3. Mô tả đặc trưng:***
  + Feature Engineering:
    + Nhãn của đối tượng trong Bounding Box: [0: With helmet, 1: Without helmet]
    + Tọa độ trung tâm của Bounding Box $(b_x,b_y)$
    + Chiều rộng của Bounding Box $(b_w)$
    + Chiều cao của Bounding Box $(b_h)$
  + [Dataset](https://drive.google.com/drive/folders/19v54FWdpb9INrt_XVGWOd2mek_REzPg1?usp=sharing)
  
***4. Mô tả về thuật toán máy học:***
  + Sử dụng mô hình [YOLOv5](https://github.com/ultralytics/yolov5): YOLOv5 là một mô hình Object Detection thuộc họ mô hình YOLO
  + YOLO là một thuật toán Single Stage Detector (Single Shot Detector), nghĩa là chúng sẽ dự đoán nhãn và vị trí của đối tượng trong toàn bộ bức ảnh chỉ với một lần chạy thuật toán duy nhất. Và tất nhiên, cách làm việc này giúp cho thời gian xử lý của YOLO rất nhanh, phù hợp với các ứng dụng cần chạy Realtime.
  + YOLO là một mô hình mạng neural tích chập (CNN) dùng cho việc phát hiện, nhận dạng, phân loại đối tượng. YOLO được tạo ra từ việc kết hợp giữa các lớp phức tạp (convolutional layers) cho phép trích xuất ra các đặc tính của ảnh và lớp kết nối (connected layers) dự đoán ra xác suất xuất hiện và tọa độ của đối tượng.
  + Cách làm viêc của YOLO có thể tóm tắt như sau:
    + Chia bức ảnh thành các Cells. Ví dụ: 19x19, 13x13, …
    + Mỗi Cell chịu trách nhiệm dự đoán ra b Bounding Box. b = 3, 5, 7, …
    + Ouput của việc dự đoán bao gồm:
      + Tọa độ trung tâm của Bounding Box $(b_x,b_y)$
      + Chiều rộng của Bounding Box $(b_w)$
      + Chiều cao của Bounding Box $(b_h)$
      + Nhãn của đối tượng trong Bounding Box (c)
      + Xác suất có đối tượng trong Bounding Box $(p_c)$
     
***6. Đánh giá kết quả, kết luận:***
  + Theo dõi chi tiết kết quả chạy trên [Colab](https://colab.research.google.com/drive/1YNQh8u59gZKF3RymvUp_BGZdewoUNNVv)

