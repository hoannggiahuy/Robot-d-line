📌 1. Giới thiệu dự án

Robot được thiết kế nhằm mục đích:

Áp dụng kiến thức hệ thống nhúng vào mô hình thực tế

Tìm hiểu về cảm biến IR, HC-SR04, driver TB6612FNG

Sử dụng PID để điều khiển robot chạy mượt và ổn định

Minh chứng khả năng tích hợp phần cứng – phần mềm của nhóm sinh viên CNTT 17-01

Robot có thể chạy theo đường vạch đen, xử lý cua, tự động né vật cản, và quay lại đường line một cách chính xác.

⚙️ 2. Phần cứng sử dụng

Linh kiện------------Số lượng-----Ghi chú

Arduino Uno R3------1------Vi điều khiển trung tâm TCRT5000	5	Cảm biến dò line

HC-SR04	     ------1------	            Cảm biến siêu âm đo khoảng cách

TB6612FNG	       ------   1	    ------        Driver điều khiển 2 motor

Động cơ DC giảm tốc	------2------	            Motor bánh trái/phải

Khung xe robot	    ------1------	            Xe 2 bánh + 1 bánh tự do

Pin Li-ion 7.4V	    ------1------           	Nguồn chính cho hệ thống

🔌 3. Sơ đồ chân (Pin Mapping)
➤ Cảm biến IR
Cảm biến	Chân Arduino

S1->D2

S2	->D3

S3	->D4

S4	->D6

S5	->D7

➤ HC-SR04

Chân	Arduino

Trig->D8

Echo->D12

➤ Driver TB6612FNG

TB6612->Arduino

STBY->	A0

PWMA	->9

AIN1->	10

AIN2	->11

PWMB	->5

BIN1	->13

BIN2->	A1

🧮 4. Thuật toán hoạt động
✔️ 4.1. Line following (PID)

Từ dữ liệu 5 cảm biến IR, robot tính toán sai số (error) theo trọng số:

S1  S2  S3  S4  S5

-2  -1   0  +1  +2


Bộ điều khiển PID:

P = error
I = I + error (giới hạn ±50)
D = error - last_error
PID = Kp·P + Ki·I + Kd·D


Thông số tối ưu sau tuning:

Kp = 75
Ki = 0.1
Kd = 1500

✔️ 4.2. Né chướng ngại vật

Đo khoảng cách bằng HC-SR04

Nếu < 20 cm → kích hoạt chế độ né

Chuỗi né vật gồm:

STOP → Lùi → Rẽ phải → Tiến → Rẽ trái → Tìm lại line

✔️ 4.3. Mất line

Khi 5 cảm biến = 1 (11111) > 3 giây → robot tự động dừng an toàn.

💻 5. Hướng dẫn chạy mã
1. Cài đặt phần mềm:

Arduino IDE (phiên bản mới nhất)

2. Mở project
Do_Line_5Mat_PID.ino

3. Chọn board
Tools → Board → Arduino Uno

4. Chọn cổng COM
Tools → Port → COMx

5. Upload

Nhấn Upload

📊 6. Kết quả thực nghiệm
✔️ Dò line

Bám line ổn định 95%

Sai lệch trung bình < 1 cm

Vào cua mượt nhờ Kd cao

✔️ Né vật

Nhận vật ở 10–20 cm

Tỷ lệ né thành công 90%

Thời gian phản hồi ~ 1.2 giây

✔️ Mất line

Robot tự dừng an toàn sau 3 giây

Video minh hoạ nằm trong file:

7233073036312.mp4

📉 7. Hạn chế

IR dễ nhiễu khi ánh sáng mạnh

Tốc độ cao dễ lạc line

Né vật đôi khi chậm nếu vật quá gần (<10 cm)

🚀 8. Hướng phát triển

Tích hợp camera ESP32-CAM để đọc vạch bằng AI

Điều khiển robot qua WiFi/Bluetooth

Thêm màn OLED hiển thị thông số PID, distance

Nâng cấp SLAM + IMU để robot tự lập bản đồ
