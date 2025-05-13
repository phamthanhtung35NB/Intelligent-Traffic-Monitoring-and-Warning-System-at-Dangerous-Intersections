# Intelligent Traffic Monitoring and Warning System at Dangerous Intersections
# Hệ thống cảnh báo, giám sát giao thông thông minh tại các điểm giao cắt nguy hiểm
> ⚠️ **Lưu ý**:
> - Repository này hiện **không chứa mã nguồn** của hệ thống do một số lý do liên quan đến **bảo mật và quyền riêng tư** nếu có thắc mắc vui lòng liên hệ qua email tungpham010203@gmail.com.
---
## Giới thiệu

- Dự án nhằm giải quyết vấn đề tai nạn giao thông tại các điểm giao cắt nguy hiểm (bị che khuất, tầm nhìn hạn chế), bằng cách xây dựng một hệ thống cảnh báo, giám sát thông minh, tự động phát hiện phương tiện đi tới điểm giao cắt để đưa ra cảnh báo cho các phương tiện từ hướng khác **Rằng:** có phương tiện đang đi tới điểu giao cắt.
- Điểm giao cắt ở đây là điểm giao cắt có mật độ phương tiện thấp chưa cần dùng đèn gia thông (xanh-vàng_đỏ).
- Và tính toán tốc độ, cảnh báo và chụp ảnh xe vi phạm.

## Thành tích

- 🥇 **Giải Nhất cấp tỉnh Ninh Bình** Cuộc thi Khoa học kỹ thuật học sinh trung học năm học 2021-2022 
  
  [Phòng GD&ĐT TP Ninh Bình](https://ninhbinh.edu.vn/pgdtpninhbinh/cong-van-van-ban/van-ban-phong-gd-dt/thong-bao-ket-qua-cuoc-thi-khoa-hoc-ky-thuat-danh-cho-hoc-si4.html)

- 🥉 **Giải Ba cấp Quốc gia** Cuộc thi Khoa học kỹ thuật học sinh trung học năm học 2021-2022  
  Được giới thiệu trên nhiều báo:
  - [Báo Ninh Bình](https://baoninhbinh.org.vn/hai-nam-sinh-truong-thpt-hoa-lu-a-voi-giai-ba-quoc-gia-ve-sang-kien-canh-bao-giao-thong-thong-minh/d20220607082731436.htm)
  - [Báo Ninh Bình](https://baoninhbinh.org.vn/ninh-binh-co-2-du-an-dat-giai-ba-cuoc-thi-nghien-cuu-khkt/d2022032722111733.htm)
  - [Đài NBTV](https://nbtv.vn/news/401/48578/du-an-cua-hoc-sinh-truong-thpt-hoa-lu-a-dat-giai-ba-cuoc-thi-khoa-hoc-ky-thuat-quoc-gia)
  - [Tuyensinh247](https://tuyensinh247.com/bai-tap-560791.html)

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

## Công nghệ sử dụng

- **Phần cứng**: Camera Hikvision, Mini PC ASUS, Arduino Mega 2560, Module Relay 16, Đèn LED cảnh báo.
- **Phần mềm**: 
  - Python/OpenCV (xử lý ảnh).
  - Firebase (Lưu trữ dữ liệu)
  - Arduino IDE (C++) cho điều khiển thiết bị.

## Kiến trúc hệ thống

1. **Phát hiện**: Camera thu hình phương tiện.
2. **Xử lý**: Mini PC phân tích tốc độ, hướng di chuyển.
3. **Cảnh báo**: Arduino kích hoạt đèn/còi theo tình huống.

## Kết quả thử nghiệm

- Hệ thống hoạt động tốt trong nhiều kịch bản: xe vượt tốc độ, nhiều hướng giao thông.
- Cảnh báo rõ ràng, chính xác; ghi lại hình ảnh phương tiên.



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

* **Phạm Thanh Tùng** – Phụ trách chính phần **lập trình, xử lý ảnh, thuật toán tính tốc độ và điều khiển hệ thống Arduino**.
* **Hoàng Tiến Đạt** – Phụ trách phần **thiết kế, lắp ráp phần cứng, lựa chọn linh kiện và triển khai thực tế**.

Cả hai thành viên cùng tham gia nghiên cứu, xây dựng ý tưởng và thử nghiệm hệ thống.

> **Thầy hưỡng dẫn**: Nguyễn Mạnh Tú.

> **Thời gian thực hiện**: 08/2021 – 03/2022
> 
> **Cuộc thi**: Khoa học kỹ thuật cấp quốc gia, năm học 2021 – 2022

## Ghi chú & Lời cảm ơn

Chúng tôi cũng xin gửi lời cảm ơn chân thành đến cộng đồng Arduino, Python và xử lý ảnh – những người đã chia sẻ vô số tài liệu, hướng dẫn, bài viết, video và kinh nghiệm thực tiễn. Sự cởi mở và sẵn sàng chia sẻ kiến thức của cộng đồng là nền tảng giúp những người trẻ như chúng tôi tiếp cận và hiện thực hóa các ý tưởng công nghệ.

> **Lưu ý quan trọng**:
>
> * Dự án này được thực hiện **với mục đích học tập, nghiên cứu khoa học và đóng góp vào các giải pháp ứng dụng thực tiễn**.
> * **Không sử dụng cho mục đích thương mại** dưới bất kỳ hình thức nào.
> * Một số đoạn mã, thư viện và ý tưởng thuật toán trong dự án có tham khảo từ cộng đồng, chúng tôi đã có sự kế thừa và phát triển từ các nguồn mở và cá nhân có cùng đam mê.

Một lần nữa, xin chân thành cảm ơn cộng đồng kỹ thuật đã chia sẻ tri thức quý báu.

