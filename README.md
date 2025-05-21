# Intelligent Traffic Monitoring and Warning System at Dangerous Intersections
# Hệ thống cảnh báo, giám sát giao thông thông minh tại các điểm giao cắt nguy hiểm
> ⚠️ **Lưu ý**:
> - Repository này hiện **không chứa mã nguồn** của hệ thống do một số lý do liên quan đến **bảo mật và quyền riêng tư của cuộc thi** nếu có thắc mắc vui lòng liên hệ qua email tungpham010203@gmail.com.
---


# Thành tích - Các bài báo

- 🥉 **Giải Ba cấp Quốc gia** Cuộc thi Khoa học kỹ thuật học sinh trung học năm học 2021-2022  

  **Link:**
  - [Báo Ninh Bình](https://baoninhbinh.org.vn/hai-nam-sinh-truong-thpt-hoa-lu-a-voi-giai-ba-quoc-gia-ve-sang-kien-canh-bao-giao-thong-thong-minh/d20220607082731436.htm)
  - [Đài NBTV](https://nbtv.vn/news/401/48578/du-an-cua-hoc-sinh-truong-thpt-hoa-lu-a-dat-giai-ba-cuoc-thi-khoa-hoc-ky-thuat-quoc-gia)
  - [Báo Ninh Bình](https://baoninhbinh.org.vn/ninh-binh-co-2-du-an-dat-giai-ba-cuoc-thi-nghien-cuu-khkt/d2022032722111733.htm)
  - [Tuyensinh247](https://tuyensinh247.com/bai-tap-560791.html)
  - [Báo NBTV](https://nbtv.vn/news/401/26640/sang-kien-huu-ich-dam-bao-atgt-tai-diem-giao-cat-nguy-hiem)

- 🥇 **Giải Nhất cấp tỉnh Ninh Bình** Cuộc thi Khoa học kỹ thuật học sinh trung học năm học 2021-2022

  **Link:**
  - [Báo Phòng GD&ĐT TP Ninh Bình](https://ninhbinh.edu.vn/pgdtpninhbinh/cong-van-van-ban/van-ban-phong-gd-dt/thong-bao-ket-qua-cuoc-thi-khoa-hoc-ky-thuat-danh-cho-hoc-si4.html)
  - [Báo Dân Sinh](https://dansinh.dantri.com.vn/dien-dan-dan-sinh/68-du-an-doat-giai-khoa-hoc-ki-thuat-tinh-ninh-binh-20220102224339000.htm)


## Giới thiệu

- Dự án nhằm giải quyết vấn đề tai nạn giao thông tại các điểm giao cắt nguy hiểm (bị che khuất, tầm nhìn hạn chế), bằng cách xây dựng một hệ thống cảnh báo, giám sát thông minh, tự động phát hiện phương tiện đi tới điểm giao cắt để đưa ra cảnh báo cho các phương tiện từ hướng khác **Rằng:** có phương tiện đang đi tới điểu giao cắt.
- Điểm giao cắt ở đây là điểm giao cắt có mật độ phương tiện thấp chưa cần dùng đèn gia thông (xanh-vàng_đỏ).
- Và tính toán tốc độ, cảnh báo và chụp ảnh xe vi phạm.


# Chi tiết về công nghệ sử dụng trong hệ thống của dự án:

## Phần mềm

### 1. Python/OpenCV
- **Mô tả**: Nền tảng xử lý hình ảnh và phát hiện đối tượng chính của hệ thống.
- **Thành phần và tính năng**:
  - **OpenCV**: Thư viện Computer Vision mã nguồn mở
    - Hỗ trợ thuật toán trừ nền (BackgroundSubtractorMOG)
    - Cung cấp các hàm xử lý hình ảnh như threshold, morphologyEx, erode
    - Hỗ trợ tìm và xử lý đường viền (contours)
    - Vẽ hình chữ nhật, đường thẳng để đánh dấu đối tượng và vùng tính toán
  
  - **Lấy dữ liệu từ camera**:
    ```python
    # Đọc video từ camera IP
    cap = cv2.VideoCapture('rtsp://admin:abcd1234@192.168.0.109/8000', cv2.CAP_FFMPEG)
    ```

### 2. Firebase
- **Mô tả**: Nền tảng đám mây để lưu trữ dữ liệu và hình ảnh vi phạm.
- **Thành phần và tính năng**:
  - **Realtime Database**: Cơ sở dữ liệu thời gian thực để lưu thông tin vi phạm
  - **Cloud Storage**: Lưu trữ hình ảnh phương tiện vi phạm tốc độ
  - **Kết nối thông qua thư viện Pyrebase**:


### 3. Arduino IDE (C/C++)
- **Mô tả**: Phần mềm lập trình cho vi điều khiển Arduino để điều khiển hệ thống cảnh báo vật lý.
- **Thành phần và tính năng**:
  - **Giao tiếp Serial**: Nhận dữ liệu từ Mini PC qua cổng COM
    ```python
    # Kết nối với Arduino qua cổng COM6, tốc độ 115200 baud
    ser = serial.Serial('COM6', 115200, timeout=0)
    
    # Gửi dữ liệu tốc độ tới Arduino
    send_vt = str(vantoc) + '\n'
    ser.write(bytes(send_vt, 'utf-8'))
    ```
  
  - **Điều khiển đèn cảnh báo**: Dựa trên dữ liệu tốc độ nhận được từ Mini PC
    - Kích hoạt đèn báo khi phát hiện phương tiện đang tiến đến điểm giao cắt
    - Thay đổi mức độ cảnh báo (tần số nhấp nháy, cường độ) dựa trên tốc độ của phương tiện
    - Tắt đèn cảnh báo sau một khoảng thời gian nhất định hoặc khi phương tiện đã đi qua điểm giao cắt
  
  - **Điều khiển còi cảnh báo**: Kích hoạt âm thanh cảnh báo khi cần thiết (có phương tiện đi với tốc độ cao tới giao cắt)
    - Điều chỉnh thời gian cảnh báo phù hợp với tình huống giao thông

## Phần cứng

### 1. Camera Hikvision
- **Mô tả**: Camera IP công nghiệp được sử dụng để giám sát điểm giao cắt. 
- **Đặc điểm kỹ thuật**:
  - Độ phân giải cao, hỗ trợ ghi hình cả ngày lẫn đêm
  - Được kết nối qua giao thức RTSP (Real Time Streaming Protocol): `rtsp://admin:abcd1234@192.168.0.109/8000`
  - Góc nhìn rộng để bao quát toàn bộ điểm giao cắt
  - Khả năng chịu điều kiện thời tiết khác nhau khi lắp đặt ngoài trời
  - Hỗ trợ nén video H.264/H.265 giúp giảm băng thông khi truyền dữ liệu

### 2. Mini PC ASUS
- **Mô tả**: Máy tính nhỏ gọn, tiết kiệm năng lượng đảm nhiệm vai trò xử lý hình ảnh và chạy thuật toán phát hiện phương tiện.
- **Cấu hình**:
  - CPU đủ mạnh để xử lý thuật toán trừ nền và theo dõi đối tượng thời gian thực
  - RAM tối thiểu 4GB để đảm bảo xử lý video mượt mà
  - Kết nối mạng LAN/WiFi để nhận dữ liệu từ camera và truyền lên Firebase
  - Cổng COM/USB để giao tiếp với Arduino
  - Hệ điều hành Windows

### 3. Arduino Mega 2560
- **Mô tả**: Vi điều khiển đóng vai trò trung gian giữa phần mềm xử lý hình ảnh và hệ thống cảnh báo vật lý.
- **Đặc điểm kỹ thuật**:
  - Microcontroller ATmega2560
  - 54 chân I/O số (trong đó 15 chân có thể xuất xung PWM)
  - 16 chân đầu vào analog
  - 4 UART (cổng nối tiếp phần cứng)
  - Tốc độ xung nhịp 16 MHz
  - Kết nối với Mini PC qua cổng COM6 với tốc độ baud 115200
  - Chịu trách nhiệm điều khiển đèn cảnh báo và còi thông qua module relay

### 4. Module Relay 16 kênh
- **Mô tả**: Mạch chuyển mạch điện để điều khiển các thiết bị cảnh báo công suất lớn.
- **Đặc điểm kỹ thuật**:
  - 16 kênh relay độc lập
  - Điện áp điều khiển 5V (tương thích với Arduino)
  - Ngõ ra tiếp điểm thường mở (NO) và thường đóng (NC)
  - Khả năng chịu tải 10A/250VAC hoặc 10A/30VDC
  - Đèn LED báo trạng thái cho mỗi kênh
  - Kích hoạt mức logic thấp (active low)

### 5. Đèn LED cảnh báo
- **Mô tả**: Hệ thống đèn cảnh báo được lắp đặt tại điểm giao cắt để thông báo cho người tham gia giao thông.
- **Đặc điểm kỹ thuật**:
  - Đèn LED công suất lớn, ánh sáng mạnh, dễ nhận biết trong nhiều điều kiện thời tiết
  - Có thể nhấp nháy với tần số khác nhau tùy vào mức độ cảnh báo
  - Màu sắc khác nhau cho các mức độ cảnh báo (vàng cho cảnh báo thông thường, đỏ cho cảnh báo khẩn cấp)
  - Được điều khiển qua module relay từ Arduino

## Tích hợp hệ thống

- **Luồng dữ liệu**:
  1. Camera ghi hình điểm giao cắt và truyền video theo thời gian thực tới Mini PC
  2. Mini PC xử lý video để phát hiện phương tiện và tính toán tốc độ
  3. Khi phát hiện phương tiện, Mini PC gửi thông tin tới Arduino qua cổng COM
  4. Arduino điều khiển đèn và còi cảnh báo thông qua Module Relay
  5. Nếu phát hiện phương tiện vi phạm tốc độ, hình ảnh được lưu vào bộ nhớ và tải lên Firebase

- **Vận hành tự động**:
  - Hệ thống hoạt động liên tục và tự động 24/7
  - Tự động điều chỉnh mức độ cảnh báo dựa trên tình huống giao thông
  - Ghi nhận vi phạm và lưu trữ dữ liệu không cần sự can thiệp của con người

- **Tiết kiệm năng lượng**:
  - Hệ thống chỉ kích hoạt cảnh báo khi phát hiện phương tiện
  - Đèn và còi cảnh báo được tắt sau khi phương tiện đã đi qua điểm giao cắt
  - Sử dụng Mini PC và Arduino có mức tiêu thụ điện năng thấp


## Chi tiết về thuật toán phát hiện phương tiện tại điểm giao cắt


### 1. Nguyên lý hoạt động tổng quát

Hệ thống sử dụng kỹ thuật **trừ nền (background subtraction)** kết hợp với **theo dõi đối tượng (object tracking)** để phát hiện và giám sát phương tiện tiếp cận điểm giao cắt. Quá trình này gồm các bước chính:

1. Trừ nền để tách phương tiện di chuyển khỏi nền cố định
2. Xác định và đánh dấu đối tượng di chuyển
3. Theo dõi đối tượng qua các khung hình liên tiếp
4. Gán ID và tính toán các thông số (tốc độ, vị trí)

### 2. Phát hiện chuyển động bằng trừ nền

Sử dụng các thuật toán trừ nền từ OpenCV:

Thuật toán MOG (Mixture Of Gaussians) tạo ra mô hình nền dựa trên sự phân phối Gaussian. Khi có đối tượng di chuyển, pixel tại vị trí đó sẽ khác biệt so với mô hình nền, tạo ra "mặt nạ" (mask) chỉ ra các khu vực có chuyển động.

```python
fgmask = fgbg.apply(frame)  # Áp dụng trừ nền để tạo mặt nạ chuyển động
ret, imBin = cv2.threshold(fgmask, 100, 255, cv2.THRESH_BINARY)  # Phân ngưỡng để có hình ảnh nhị phân
```

### 3. Xử lý hình ảnh để loại bỏ nhiễu

Sau khi có mặt nạ nhị phân, hệ thống áp dụng các kỹ thuật hình thái học để lọc nhiễu và cải thiện chất lượng phát hiện:

```python
# Xử lý hình thái học để cải thiện chất lượng phát hiện
mask1 = cv2.morphologyEx(imBin, cv2.MORPH_OPEN, kernalOp)  # Mở - loại bỏ điểm nhiễu nhỏ
mask2 = cv2.morphologyEx(mask1, cv2.MORPH_CLOSE, kernalCl)  # Đóng - làm đầy các lỗ hổng nhỏ
e_img = cv2.erode(mask2, kernal_e)  # Xói mòn - làm mỏng các đường viền
```

- **MORPH_OPEN**: Loại bỏ các chấm nhiễu nhỏ khỏi mặt nạ
- **MORPH_CLOSE**: Làm đầy các lỗ hổng nhỏ trong đối tượng
- **Erode**: Làm mỏng đường viền các đối tượng

### 4. Tìm đường viền và xác định phương tiện

Sau khi có mặt nạ đã được xử lý, hệ thống tìm các đường viền (contours) - đây là các vùng liên tục đại diện cho các đối tượng:

```python
contours, _ = cv2.findContours(e_img, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
```

Hệ thống lọc các đường viền dựa trên diện tích để tránh phát hiện các vật thể quá nhỏ:

```python
detections = []
for cnt in contours:
    area = cv2.contourArea(cnt)
    if area > 1400:  # Chỉ xét các đối tượng có diện tích > 1400 pixel
        x, y, w, h = cv2.boundingRect(cnt)  # Tính hình chữ nhật bao quanh đối tượng
        if w > 4:  # Đảm bảo chiều rộng đủ lớn
            cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 3)  # Vẽ hình chữ nhật
            detections.append([x, y, w, h])  # Thêm vào danh sách phát hiện
```

### 5. Theo dõi đối tượng và gán ID

#### Bắt đầu theo dõi đối tượng qua các frame. Nguyên lý hoạt động:

Mỗi khi phát hiện được đối tượng mới, hệ thống sẽ:
1. Cập nhật vị trí của các đối tượng đã được theo dõi
2. Gán ID mới cho đối tượng chưa được theo dõi
3. Duy trì danh sách các đối tượng đang trong vùng giám sát

### 6. Xác định vị trí và tính toán tốc độ

Hệ thống sử dụng hai đường kẻ ngang trên khung hình để tính thời gian di chuyển của phương tiện:

```python
# Vẽ hai đường tham chiếu để tính tốc độ
cv2.line(frame, (0, 220), (4600, 220), (0, 0, 255), 2)  # Đường tham chiếu 1
cv2.line(frame, (0, 165), (4600, 165), (0, 0, 255), 2)  # Đường tham chiếu 2
```

Khi phương tiện đi qua đường tham chiếu thứ nhất (y=220), hệ thống bắt đầu đếm thời gian. Khi phương tiện đi qua đường tham chiếu thứ hai (y=165), hệ thống dừng đếm thời gian và tính tốc độ:

```python
def getSpeed(self, id):
    if (self.s[0, id] > 0):
        s = (0.83 / self.s[0, id]) * 3.6  # km/h - 0.83 là khoảng cách thực tế giữa hai đường (m)
    else:
        s = 0
    return int(s)
```

### 7. Cảnh báo và ghi nhận vi phạm
- Nếu tốc độ vượt quá giới hạn, hệ thống sẽ đánh dấu phương tiện bằng màu khác và chụp ảnh lưu lại.


## Tiêu chí của hệ thống:
- Tự động phát hiện phương tiện đi tới điểm giao cắt để đưa ra cảnh báo cho các phương tiện từ hướng khác **Rằng:** có phương tiện đang đi tới điểu giao cắt. (Điểm giao cắt ở đây là điểm giao cắt có mật độ phương tiện thấp chưa cần dùng đèn gia thông (xanh-vàng_đỏ))

- Giám sát bằng cách ước tính tốc độ, chụp hình phương tiện đi tới điểm giao cắt với tốc độ cao khoảng (60 km/h) đây là tốc độ hệ thống đang đặt, có thể tuỳ chỉnh.

## Tính năng chính

* **Phát hiện phương tiện ở các hướng đến điểm giao cắt**:
  Sử dụng thuật toán so sánh chuyển động giữa 2 frame liên tiếp để phát hiện sự thay đổi vị trí của vật thể. Mỗi phương tiện được đánh số ID riêng biệt để theo dõi trong toàn bộ quá trình di chuyển.

* **Tính toán tốc độ phương tiện**:

  * Vẽ hai đường kẻ ngang cố định trên khung hình (đường 1 và đường 2) tương ứng với một đoạn đường có độ dài thực tế $s$ (đơn vị mét).
  * Khi một phương tiện di chuyển từ đường kẻ 1 đến đường kẻ 2, hệ thống ghi lại thời gian $t$ (giây) mà phương tiện đi qua đoạn đường này.
  * Áp dụng công thức vật lý: v = s\t để tính tốc độ gần đúng của phương tiện. (phục vụ để tính thời gian tắt đèn cảnh báo khi xe đã qua điểm giao cắt và xử phạt tốc độ cao)

* **Cảnh báo và giám sát thông minh**:

  * Phát cảnh báo bằng đèn và còi khi phát hiện phương tiện đang tiến đến điểm giao cắt nguy hiểm.
  * Nếu phương tiện vượt quá tốc độ cho phép, hệ thống sẽ:

    * Kích hoạt cảnh báo nâng cao (đèn/còi mạnh hơn).
    * Tự động chụp ảnh phương tiện vi phạm.
    * Gửi ảnh và dữ liệu vi phạm thời gian thực lên **Firebase** để lưu trữ và hỗ trợ giám sát từ xa.


## Kết quả thử nghiệm

- Hệ thống hoạt động tốt trong nhiều kịch bản: xe vượt tốc độ, nhiều hướng giao thông.
- Cảnh báo rõ ràng, chính xác; ghi lại hình ảnh phương tiên.

### Ưu điểm của phương pháp này

1. **Đơn giản và hiệu quả**: Sử dụng thuật toán trừ nền tiêu chuẩn từ OpenCV
2. **Xác định độc lập từng phương tiện**: Mỗi phương tiện được đánh ID riêng biệt
3. **Ít tài nguyên**: Không yêu cầu GPU hay phần cứng mạnh
4. **Tính toán tốc độ thực tế**: Dựa trên thời gian di chuyển qua khoảng cách đã biết
5. **Tích hợp với hệ thống cảnh báo**: Gửi dữ liệu qua Arduino để kích hoạt cảnh báo

### Hạn chế

1. **Nhạy cảm với thay đổi ánh sáng**: Thuật toán trừ nền có thể bị ảnh hưởng bởi điều kiện ánh sáng
2. **Chồng lấp đối tượng**: Khó phân biệt khi nhiều phương tiện chồng lấp nhau
3. **Điều kiện thời tiết**: Hiệu suất có thể giảm trong điều kiện mưa, tuyết, sương mù
4. **Góc camera**: Phụ thuộc nhiều vào vị trí đặt camera


## Một số hình ảnh thử nghiệm trên camera được lắp trước điểm giao cắt trước cổng trường học:
#### Do các hình ảnh và video lưu trữ đã lâu nên chất lượng hình ảnh có thể đã bị giảm

<p align="center">
  <img src="https://github.com/user-attachments/assets/56e60d84-c97f-4281-8da5-cf9b29b8811b" width="45%">
  <img src="https://github.com/user-attachments/assets/9f560e7e-b493-4b10-b923-756669f81327" width="45%">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/9da2bc39-e984-4f9a-9381-1a49da8fd1a7" width="45%">
  <img src="https://github.com/user-attachments/assets/eb8139f8-5b63-4cb1-9288-5ab9569a2c5c" width="45%">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/72c7f747-afbc-4d73-974b-f1d5e0d20081" width="45%">
  <img src="https://github.com/user-attachments/assets/b886621e-18ba-4149-8c62-8d569e2398ef" width="45%">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/cff0c563-88cd-467d-bded-68344dcbeb1c" width="45%">
  <img src="https://github.com/user-attachments/assets/020c3634-92c6-4691-a31c-f642f4c1b265" width="45%">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/b20f140f-8a7d-4424-82e6-e450384bc5f7" width="45%">
  <img src="https://github.com/user-attachments/assets/951684bd-62d5-40df-be46-058d41661e36" width="45%">
</p>




> **Tác giả**: Nhóm học sinh Trường THPT Hoa Lư A, Ninh Bình (tôi: Phạm Thanh Tùng - Hoàng Tiến Đạt)

>* **Phạm Thanh Tùng** – Phụ trách chính phần **lập trình, xử lý ảnh, thuật toán tính tốc độ và điều khiển hệ thống Arduino**.
>* **Hoàng Tiến Đạt** – Phụ trách phần **thiết kế, lắp ráp phần cứng, lựa chọn linh kiện và triển khai thực tế**.

Cả hai thành viên cùng tham gia nghiên cứu, xây dựng ý tưởng và thử nghiệm hệ thống.

> **Thầy hưỡng dẫn**: Nguyễn Mạnh Tú.
>
> **Thời gian thực hiện**: 08/2021 – 03/2022
> 
> **Cuộc thi**: Khoa học kỹ thuật cấp quốc gia, năm học 2021 – 2022

## Ghi chú & Lời cảm ơn

Chúng tôi cũng xin gửi lời cảm ơn chân thành đến cộng đồng Arduino, Python và xử lý ảnh – những người đã chia sẻ vô số tài liệu, hướng dẫn, bài viết, video và kinh nghiệm thực tiễn. Sự cởi mở và sẵn sàng chia sẻ kiến thức của cộng đồng là nền tảng giúp những người trẻ như chúng tôi tiếp cận và hiện thực hóa các ý tưởng công nghệ.

> **⚠️⚠️**
> * Dự án này được thực hiện **với mục đích học tập, nghiên cứu khoa học và đóng góp vào các giải pháp ứng dụng thực tiễn**.
> * **Không sử dụng cho mục đích thương mại** dưới bất kỳ hình thức nào.
> * Một số đoạn mã, thư viện và ý tưởng thuật toán trong dự án có tham khảo từ cộng đồng, chúng tôi đã có sự kế thừa và phát triển từ các nguồn mở và cá nhân có cùng đam mê.

Một lần nữa, xin chân thành cảm ơn cộng đồng kỹ thuật đã chia sẻ tri thức quý báu.

