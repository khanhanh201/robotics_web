---
id: toggle
title: Chuyển trạng thái (Toggle)
sidebar_position: 2
---

## Nhắc lại về tín hiệu digital
- Trong hầu hết các điều khiển, ngoại trừ 2 joystick sẽ chỉ còn những nút bấm cho ra tín hiệu digital (rời rạc: nhấn = 1, không nhấn = 0).
- Do có 2 trạng thái, ta có thể sử dụng kiểu tín hiệu này để điều khiển trạng thái bật/tắt của động cơ. Chẳng hạn như khi nút được nhấn thì động cơ chạy, nút khong được nhấn thì tắt động cơ. Dưới đây là pseudocode điều khiển động cơ theo cách nói trên:

```jsx title = ""
void opcontrol() {
  while (true) {
    if (Controller.ButtonL1.pressing()) {
      // Nếu nhấn nút L1 thì động cơ quay
      Motor1.setVelocity(100, percent);
      Motor1.spin(forward);
    } else {
      // Nếu thả nút L1 thì động cơ dừng
      Motor1.stop();
    }

    wait(20, msec);
  }
}
```
:::caution[Nhược điểm]
Cách vừa rồi (nhấn → chạy, nhả → dừng) tuy dễ hiểu nhưng có những hạn chế:
- 🚫 Phải giữ nút liên tục: Nếu muốn động cơ chạy lâu (ví dụ motor bắn bóng), người điều khiển phải giữ nút không ngừng → dễ mỏi tay.
- 🚫 Khó kết hợp nhiều thao tác: Khi vừa phải giữ nút cho một cơ cấu, vừa điều khiển khung gầm, người lái sẽ bị phân tâm.
:::

## Toggle
### 1. Giới thiệu
Để khắc phục những nhược điểm nói trên, ta dùng kỹ thuật toggle (bật/tắt theo nút nhấn):
- Nhấn 1 lần → động cơ chạy (bật).
- Nhấn thêm lần nữa → động cơ tắt.
- Người điều khiển không cần giữ nút, mà chỉ cần nhấn để chuyển trạng thái. Có thể tưởng tượng toggle như công tắc đèn: bật thì đèn sáng, nhấn lại thì đèn tắt

### 2. Cấu trúc lập trình toggle
- Biến trạng thái (state variable): Sử dụng biến boolean hoặc int để lưu trạng thái hiện tại (ON/OFF).
- Điều kiện chuyển đổi: khi phát hiện nút được nhấn, đổi giá trị của biến.

Ví dụ pseudocode:
```jsx title = "Toggle"
bool running = false;
void opcontrol() {
  while (true) {
    if (Controller.ButtonL1.pressing()) {
      // Nếu nhấn nút L1 thì đảo trạng thái của biến running
      running = !running
    } 
    if(running){
        // Nếu biến running đúng, chạy động cơ
        Motor1.setVelocity(100, percent);
        Motor1.spin(forward);
    }
    else{
        //Ngược lại, tắt động cơ
        Motor1.stop();
    }

    wait(20, msec);
  }
}
```

### 3. Ứng dụng nâng cao
#### a. Kết hợp bật/tắt và thay đổi chiều quay
- Trong cấu trúc lập trình toggle cơ bản, ta chỉ có thể tắt hoặc chạy động cơ theo 1 chiều và chưa chạy được động cơ theo chiều còn lại.
- Nếu có 1 chiều cần chạy liên tục và 1 chiều còn lại chỉ thỉnh thoảng mới sử dụng đến, ta có thể sử dụng 1 nút để toggle bật/tắt động cơ và nhấn giữ 1 nút khác để quay ngược động cơ (bất kể động cơ đang quay thuận / dừng).
Ví dụ pseudocode:
```jsx title = "Toggle with reversed motor"
bool running = false;
void opcontrol() {
  while (true) {
    if (Controller.ButtonL2.pressing()) {
      // Nếu nhấn giữ nút L2 thì quay ngược động cơ lại bất kể động cơ đang bật hay tắt
      Motor1.setVelocity(-100, percent);
      Motor1.spin(forward);
    } 
    else{
        // Còn lại, tương tự như cấu trúc toggle cơ bản
        if (Controller.ButtonL1.pressing()){
            running = !running;
        }
        if(running){
            Motor1.setVelocity(100, percent);
            Motor1.spin(forward);
        }
        else{
            Motor1.stop();
        }
    }

    wait(20, msec);
  }
}
```
#### b. Chuyển đổi giữa nhiều trạng thái
- Ví dụ robot có 3 mức tốc độ/trạng thái → mỗi lần nhấn nút sẽ chuyển sang tốc độ/trạng thái tiếp theo (Low → Medium → High → Low...).
- Khi đó, thay vì sử dụng biến bool để lưu trạng thái, ta sử dụng kiểu biến enum (danh sách), cho phép ta lưu từ 3 trạng thái trở lên. Kết hợp với câu lệnh rẽ nhánh (if-else hoặc switch - case), ta có thể quyết định hành động của robot trong từng trường hợp.
