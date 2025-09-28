---
id: control-schemes
title: Các kiểu điều khiển
sidebar_position: 1
---
## Tank drive
Đây là 1 cách điều khiển [khung gầm tiêu chuẩn](/mechanics/drivetrain.md) **dễ hiểu và đơn giản**. Chúng ta sẽ điều khiển độc lập tốc độ và chiều di chuyển của mỗi bên khung gầm. Cụ thể, Tank drive dùng hai cần joystick để điều khiển hai bên khung gầm:

- Joystick bên trái → điều khiển bánh bên trái.
- Joystick bên phải → điều khiển bánh bên phải.

<figure style = {{ textAlign: "center"}}>
  <img src="/img/tank_drive_iq.png" width="400"/>
  <figcaption>Minh họa Tank drive trên phần mềm VEXcode IQ</figcaption>
</figure>

:::note
Ví dụ khi di chuyển:
- Khi đẩy cả hai joystick lên trước → cả hai bên bánh quay cùng chiều, robot đi thẳng tới.

- Khi kéo cả hai joystick xuống sau → robot đi lùi.

- Khi chỉ đẩy joystick trái lên, joystick phải để giữa → chỉ bánh trái quay, robot xoay lệch sang phải.

- Khi đẩy joystick trái lên, joystick phải kéo xuống → hai bên quay ngược chiều nhau, robot sẽ xoay tại chỗ.
:::


Dưới đây là đoạn pseudocode điều khiển robot bằng cách điều khiển **Tank drive** sử dụng tay cầm của cuộc thi VEX V5 (chi tiết tay cầm xem lại ở phần [Điều khiển thủ công](./intro_uc.md)).

```jsx title = "Tank drive"
void tank_drive() {
    while(true){
        float left_velocity = Controller.Axis3.position();
        float right_velocity = controller.Axis2.position();

        Left_Motor.setVelocity(left_velocity, percent);
        Right_Motor.setVelocity(right_velocity, percent);
        Left_Motor.spin(forward);
        Right_Motor.spin(forward);
    }
}
```

## Arcade drive
Arcade drive là cách điều khiển robot với [khung gầm tiêu chuẩn](/mechanics/drivetrain.md) **phổ biến nhất** vì nó **trực quan và dễ dùng**. Thay vì phải điều khiển riêng lẻ từng bên khung gầm như Tank drive, Arcade drive cho phép chúng ta kết hợp chuyển động tiến/lùi và quay trái/phải chỉ bằng joystick. Cụ thể:

- Một trục joystick (thường là trục dọc) điều khiển tốc độ tiến/lùi.

- Một trục joystick khác (thường là trục ngang) điều khiển tốc độ quay/rẽ.

- Hai giá trị này sẽ được cộng/trừ để tính ra tốc độ cho từng bên của khung gầm.


<div style={{ display: "flex", justifyContent: "center", gap: "20px" }}>
  <figure style={{ textAlign: "center" }}>
    <img src="/img/single_arcade_iq.png" alt="Ảnh 1" />
    <figcaption>Single Arcade drive</figcaption>
  </figure>
  
  <figure style={{ textAlign: "center" }}>
    <img src="/img/split_arcade_iq.png" alt="Ảnh 2"/>
    <figcaption>Split Arcade drive</figcaption>
  </figure>
</div>
Trên đây là hình minh họa cho 2 biến thể của Arcade drive. Single Arcade drive có thể điều khiển cho robot tiến, lùi và quay 2 phía chỉ bằng 1 joystick, trong khi Split Arcade drive sẽ tách trục dùng để quay robot sang joystick còn lại.

:::note
Ví dụ khi di chuyển (Trường hợp của Singe Arcade drive)
- Nếu đẩy joystick lên → robot tiến thẳng.

- Nếu kéo joystick xuống → robot lùi thẳng.

- Nếu đẩy joystick sang phải → robot quay phải.

- Nếu vừa đẩy lên vừa đẩy sang phải → robot vừa tiến vừa rẽ phải (di chuyển theo đường cong).
:::

Dưới đây là đoạn pseudocode điều khiển robot bằng cách điều khiển **Split Arcade drive** sử dụng tay cầm của cuộc thi VEX V5 (chi tiết tay cầm xem lại ở phần [Điều khiển thủ công](./intro_uc.md)).

```jsx title = "Split Arcade drive"
void arcade_drive() {
    while(true){
        float left_velocity = Controller.Axis3.position() + Controller.Axis1.position();
        float right_velocity = controller.Axis3.position() - Controller.Axis1.position();

        Left_Motor.setVelocity(left_velocity, percent);
        Right_Motor.setVelocity(right_velocity, percent);
        Left_Motor.spin(forward);
        Right_Motor.spin(forward);
    }
}
```

## Mecanum drive
:::info
Về bản chất toán học sâu xa, tham khảo thêm tại video: https://www.youtube.com/watch?v=0k-Ey9bS9lE
:::

Mecanum drive là cách điều khiển robot sử dụng [khung gầm mecanum](/mechanics/drivetrain.md). Đặc điểm nổi bật của Mecanum drive là cho phép robot di chuyển đa hướng: tiến, lùi, rẽ, và **cả đi ngang (strafe)** - điều mà khung gầm tiêu chuẩn không làm được. Nhờ đó, robot có tính linh hoạt cao và có thể di chuyển theo bất kì hướng nào mà không cần xoay thân trước.
Cách điều khiển Mecanum drive thường sử dụng cả 2 joystick:
- Trục dọc để điều khiển tốc độ tiến/lùi.
- Trục ngang (thường cùng joystick với trục dọc) để điều khiển tốc độ đi ngang (strafe).
- Một trục khác (thường là của joystick khác với 2 trục trên) để điều khiển tốc độ quay trái/phải.

<figure style = {{ textAlign: "center"}}>
  <img src="/img/Mecanum_wheel_control_principle.svg.png"/>
  <figcaption>Nguồn: [Mecanum wheeel - Wikipedia](https://en.wikipedia.org/wiki/Mecanum_wheel) </figcaption>
</figure>

Ba giá trị này được kết hợp lại để tính tốc độ cho từng bánh xe, nhờ đó robot có thể trượt chéo, xoay tại chỗ hoặc di chuyển theo đường cong một cách mượt mà.

Dưới đây là đoạn pseudocode điều khiển robot bằng cách điều khiển **Mecanum drive** sử dụng tay cầm của cuộc thi VEX V5 (chi tiết tay cầm xem lại ở phần [Điều khiển thủ công](./intro_uc.md)).

```jsx title = "Mecanum drive"
void mecanum_drive() {
    while(true){
        // Lấy giá trị của 3 trục (y: trục dọc, x: trục ngang, rx: trục thứ 3)
        float y = Controller.Axis3.position();
        float rx = Controller.Axis1.positon();
        float x = Controller.Axis4.position();
        
        // Tính giá trị
        float front_left_velocity = y + rx + x;
        float rear_left_velocity = y + rx - x;
        float front_right_velocity = y - rx - x;
        float rear_right_velocity = y - rx + x;

        // Đảm bảo tốc độ cao nhất của các động cơ là 100
        float denominator = max(abs(y) + abs(rx) + abs(x), 100) / 100;
        
        //Tính tốc độ của mỗi động cơ
        front_left_velocity /= denominator;
        rear_left_velocity /= denominator;
        front_right_velocity /= denominator;
        rear_right_velocity /= denominator;

        Front_Left_Motor.setVelocity(front_left_velocity, percent);
        Rear_Left_Motor.setVelocity(rear_left_velocity, percent);
        Front_Right_Motor.setVelocity(front_right_velocity, percent);
        Rear_Right_Motor.setVelocity(rear_right_velocity, percent);

        Front_Left_Motor.spin(forward);
        Rear_Left_Motor.spin(forward);
        Front_Right_Motor.spin(forward);
        Rear_Right_Motor.spin(forward);
    }
}
```