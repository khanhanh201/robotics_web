---
id: pid
title: PID
sidebar_position: 2
---

## Giới thiệu
- Mặc dù [Bang Bang controller](./bang_bang.md) có khả năng hiệu chỉnh nếu có sai số, nhưng lại có điểm yếu lớn là robot thường bị rung giật (do hiệu chỉnh với mức độ bằng nhau bất kể sai số lớn hay nhỏ), khó ổn định vị trí và mất rất nhiều thời gian để ổn định sau khi đã tới đích đến.
- Để khắc phục những nhược điểm nói trên, người ta thường dùng PID controller (**Proportional - Integral - Derivative** hay Tỉ lệ - Tích phân - Đạo hàm). 
- Đây là bộ điều khiển phản hồi thông minh và linh hoạt hơn, biết tự điều chỉnh tốc độ dựa trên sai số giữa vị trí hiện tại và đích đến **(error)**. Khi tiến gần vị trí đích, PID sẽ tự động giảm mức độ hiệu chỉnh để tránh bị vượt quá **(overshoot)**, và nếu có lỡ overshoot thì nó cũng có khả năng tự hiệu chỉnh quay lại.
- PID có ứng dụng rất nhiều trong một số cơ cấu Robotics như: [khung gầm](/mechanics/drivetrain.md), cánh tay, flywheel, thậm chí trong đời sống như điều khiển nhiệt độ và hơn thế nữa.

## Các thành phần của bộ điều khiển PID
:::info
Về công thức đầy đủ của bộ điều khiển PID, tham khảo thêm tại https://www.geeksforgeeks.org/electronics-engineering/proportional-integral-derivative-controller-in-control-system/
:::

Cả 3 thành phần P, I và D của bộ điều khiển PID đều là những **tham số mà con người có thể chọn trước (hyperparameter)** để điều chỉnh hành vi của hệ thống nói chung và robot nói riêng, vì thế cần chọn giá trị của P, I và D phù hợp trong từng trường hợp.

### 1. P - gain (kP)
 Trong bộ điều khiển PID, P-gain (Proportional gain) hay hệ số tỉ lệ sẽ quyết định mức độ hiệu chỉnh của robot tỉ lệ thuận với sai số hiện tại. Nói đơn giản hơn, P-gain càng lớn thì robot hiệu chỉnh mạnh hơn khi ở xa mục tiêu, và sẽ giảm tốc nhanh chóng khi robot tiến dần tới đích đến.

:::tip

- Nếu chọn P-gain quá thấp, robot sẽ di chuyển chậm chạp, thậm chí có thể không tới được mục tiêu do không đủ lực.

- Nếu chọn P-gain quá cao, robot sẽ phản ứng quá mạnh, dễ gây rung lắc hoặc overshoot qua khỏi vị trí mong muốn, từ đó mất thời gian căn chỉnh về lại điểm đích.
:::
Như vậy việc chọn giá trị của P-gain cho phù hợp là quan trọng để đảm bảo robot không di chuyển chậm chạp mà không bị rung lắc khi ở xung quanh điểm đích. Trong một số trường hợp, chỉ một mình P-gain đã đủ để robot hiệu chỉnh chính xác (hay còn gọi là P - controller).

<figure style = {{ textAlign: "center"}}>
  <img src="/img/p_controller_example.png" style={{ width: "85%", height: "auto" }} />
  <figcaption>     Biểu đồ minh họa góc hiện tại của robot theo thời gian sau khi áp dụng P-gain thích hợp <br/>
    (Đường màu xanh lá: Góc mục tiêu, Đường màu xanh lục: Góc hiện tại) </figcaption>
</figure>

<div style={{ display: "flex", justifyContent: "center", gap: "20px" }}>
  <figure style={{ textAlign: "center" }}>
    <img src="/img/p_too_small_example.png" alt="Ảnh 1" />
    <figcaption>P-gain quá nhỏ, robot không đạt được điểm đích</figcaption>
  </figure>
  
  <figure style={{ textAlign: "center" }}>
    <img src="/img/p_big_example.png" alt="Ảnh 2"/>
    <figcaption>P-gain quá lớn, robot dao động quanh điểm đích</figcaption>
  </figure>
</div>


### 2. D - gain (kD)
Trong biểu đồ minh họa góc của robot khi chỉ sử dụng P, mặc dù đã chọn giá trị thích hợp nhưng ta thấy robot vẫn dao động quanh góc mục tiêu trước khi dừng hẳn, nguyên nhân là do P - controller chỉ biết sai số hiện tại mà không "dự đoán" được xu hướng di chuyển, dẫn đến overshoot và rung lắc nhẹ. Và đây chính là lúc D phát huy tác dụng.

D-gain nhân với mức độ thay đổi của sai số hiện tại, tức là nó đo tốc độ robot đang tiến tới setpoint và phản ứng kịp thời nếu robot di chuyển quá nhanh, vì thế khi gần đạt điểm đích, nó sẽ tạo ra một "lực phanh sớm", làm giảm overshoot và dao động. Nhờ vậy, robot dừng mượt hơn, ổn định hơn mà nhanh chóng hơn do không mất thời gian cố căn chỉnh về đúng vị trí.

:::caution
Mặc dù D-gain giúp robot dừng ổn định và mượt mà hơn, nhưng nếu để D-gain quá cao, robot sẽ quá nhạy cảm với những thay đổi của sai số hiện tại dù là nhỏ nhất. Kết quả là robot chưa kịp di chuyển mượt thì đã bị phanh rất mạnh, làm cho toàn bộ chuyển động không mượt và bị rung lắc mạnh.
:::
Như vậy, với D-gain thì ta không cần phải chọn P-gain quá thấp để không bị dao động quanh điểm đích mà dẫn đến hiệu chỉnh chậm chạp. Nó vừa giúp chúng ta hiệu chỉnh nhanh hơn (P-gain cao hơn), vừa giảm đi dao động quanh điểm đích.

<figure style = {{ textAlign: "center"}}>
  <img src="/img/pd_controller_example.png" style={{ width: "85%", height: "auto" }} />
  <figcaption>     Biểu đồ minh họa góc hiện tại của robot theo thời gian sau khi áp dụng P-gain và D-gain thích hợp <br/>
    (Đường màu xanh lá: Góc mục tiêu, Đường màu xanh lục: Góc hiện tại) </figcaption>
</figure>

### 3. I - gain (kI)
Thông thường, bộ điều khiển PD đã đủ để giúp robot hiệu chỉnh một cách chính xác và ổn định (như trong biểu đồ minh họa). Tuy nhiên, do một số yếu tố ngoại cảnh như lực ma sát hay trọng lực, đôi khi robot không thể đạt điểm đích hoàn toàn trong thời gian dài, tạo ra steady-state error. Lúc này, cần thêm I-gain để loại bỏ sai số tồn đọng và giúp robot đạt điểm đích chính xác.

<figure style = {{ textAlign: "center"}}>
  <img src="/img/sse_example.png" style={{ width: "65%", height: "auto" }} />
  <figcaption>Steady - state error (robot dừng lại lâu trước khi thực sự đạt đích đến)</figcaption>
</figure>

Để khắc phục, ta thêm I-gain – nhân với tổng sai số tích lũy theo thời gian – giúp robot từ từ bù lỗi tồn đọng và đạt setpoint chính xác, đồng thời bù cho các yếu tố ngoại cảnh như ma sát hay trọng lực, làm chuyển động mượt mà hơn.

## Pseudocode
```jsx title = PID

float dt = 1e-6 //Khoảng giữa 2 bước thời gian liên tiếp
float PID (float Kp, float Ki, float Kd, float target, float measured) {
  float error = target - measured; //Cập nhật sai số hiện tại
  float proportional = Kp * error;
  integral += Ki * error * dt;
  float derivative = Kd * (error - prev_error) / dt;
  float output = proportional + integral + derivative;
  prev_error = error; //Lưu lại sai số của robot trước đó
  return output;
}
```

## Integral windup
Integral windup xảy ra khi **integral** tích lũy quá nhiều lỗi trong thời gian robot không thể đạt điểm đích, đến mức giá trị **integral** vượt quá khả năng cung cấp lực/công suất của động cơ. Khi đó:
- Robot không thể di chuyển nhanh hơn, bởi integral lớn hơn công suất motor có thể thực hiện.
- Khi robot bắt đầu di chuyển trở lại hoặc lỗi giảm, integral vẫn còn quá lớn, nên robot sẽ bị overshoot hoặc dao động mạnh.
- Robot phải đợi một khoảng thời gian để integral giảm xuống mức mà động cơ có thể kiểm soát, trước khi đạt chuyển động ổn định.

Một cách phổ biến để khắc phục integral windup là **giới hạn giá trị (clamp)** Integral, tức là giới hạn giá trị tối đa và tối thiểu mà I-term có thể tích lũy. Khi robot hoặc cơ cấu không thể đạt setpoint, I-term không được phép vượt quá mức này, nhờ đó tránh tình trạng I-term vượt công suất động cơ, gây overshoot hoặc dao động mạnh khi robot bắt đầu di chuyển trở lại. Cách này vừa đơn giản vừa hiệu quả, giúp PID vẫn loại bỏ steady-state error nhưng không tạo ra các phản ứng quá mức do lỗi tích lũy quá lớn.

:::info
Cụ thể hơn về integral windup và cách khắc phục, tham khảo thêm tại: https://www.youtube.com/watch?v=NVLXCwc8HzM
:::