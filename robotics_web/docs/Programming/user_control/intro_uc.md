---
id: intro-uc
title: User Control
---

## Giới thiệu
Trong phần hướng dẫn này, chúng ta sẽ được học cách để lập trình cho cơ chế khung gầm (drivetrain) dựa trên tín hiệu từ cần điều khiển (joystick) trên tay cầm và cách điều khiển các cơ chế ghi điểm dựa trên các nút bấm.

## Tay cầm (Controller)

### 1. Joystick
Tay cầm thông thường sẽ có 2 joystick ở mặt trước. Mỗi joystick sẽ có 2 trục, ngang và dọc và được đánh số/ký tự tùy theo tay cầm. Chúng ta sẽ sử dụng những trục này để điều khiển khung gầm.

<figure style = {{ textAlign: "center"}}>
  <img src="/img/v5_controller.png"/>
  <figcaption>Nguồn: [Using Blocks for Controller Buttons / Joysticks in VEXcode V5](https://kb.vex.com/hc/en-us/articles/360035954651-Using-Blocks-for-Controller-Buttons-Joysticks-in-VEXcode-V5) </figcaption>
</figure>

Trên đây là hình ảnh ví dụ của tay cầm được sử dụng trong cuộc thi VEX V5. Joystick bên phải có 2 trục 3 và 4 lần lượt là trục dọc, trục ngang; Joystick bên trái có 2 trục 1 và 2 lần lượt là trục ngang, trục dọc.

Chi tiết về cách sử dụng được hướng dẫn ở phần [Các kiểu điều khiển](./control_schemes.md)

### 2. Các nút bấm khác
Ngoài 2 joystick, các tay cầm thông thường luôn có nhiều nút bấm khác để điều khiển các cơ chế khác của robot. 
Với tay cầm của VEX V5, ngoài 8 nút ở trên mặt điều khiển thì còn 4 nút bấm khác ở phía trên điều khiển.
<figure style = {{ textAlign: "center"}}>
  <img src="/img/v5_controller_up.png"/>
  <figcaption>Nguồn: [Using Blocks for Controller Buttons / Joysticks in VEXcode V5](https://kb.vex.com/hc/en-us/articles/360035954651-Using-Blocks-for-Controller-Buttons-Joysticks-in-VEXcode-V5) </figcaption>
</figure>

Chi tiết về cách sử dụng được hướng dẫn ở phần [Chuyển đổi trạng thái (Toggle)](./toggle.md)

### 3. Kiểu tín hiệu
Joystick của tay cầm không chỉ có hai trạng thái bật/tắt như các nút bấm, mà nó có thể di chuyển liên tục theo trục ngang và dọc.
:::info
Như vậy joystick sẽ cho ra tín hiệu ***analog***, là kiểu tín hiệu có giá trị có thể thay đổi liên tục trên một khoảng.
:::

Các nút bấm chỉ có 2 trạng thái rời rạc (nhấn = 1, không nhấn = 0)

:::info
Đây là kiểu tín hiệu ***digital***, đơn giản và dễ hiểu, nhưng rời rạc: nó chỉ biết "có/không" hoặc "bật/tắt", không có mức độ trung gian.
:::