# Mục lục

- [Mục tiêu](#mục-tiêu)
- [Nội dung chính](#nội-dung-chính)
- [Thiết kế form chính của ứng dụng](#thiết-kế-form-chính-của-ứng-dụng)
- [Thiết kế form login của ứng dụng](#thiết-kề-form-login-cho-hệ-thống)


# Mục tiêu
Học xong phần này sinh viên có thể:
- Thiết kế được giao diện cho ứng dụng windows form
- Thiết kế form phù hợp với nhu cầu của ứng dụng

# Nội dung chính
- Thiết kế form chính của ứng dụng sử dụng menu và TabControl (Sử dụng thư viện .NetBar2).
- Thiết kế form Đăng nhập (Login)
- Cách mở form và hiển thị form theo dạng thông thường
- Cách mở form và hiển thị form theo dạng tabControl
## Thiết kế form Chính của ứng dụng
- Trong phần này chúng ta sử dụng một số control để thiết kế form chính cho ứng dụng
    - MenuStrip: Thiết kế menu cho ứng dụng
    - StatusStrip: Thanh trạng thái hiển thị trạng thái của ứng dụng
    - TabControl: sử dụng tabcontrol của thư viên DotNetBar2 để hiển thị form theo dạng tab. 

**Các bước tiến hành**:
- **Bước 1**: Đổi tên Form thành: Frm_Main
- **Bước 2**: Chỉnh các thuộc tính của form:
    + **WindowState**: Maximized
    + **StartPostion**: CenterScreen
    + **FormBorderStyle**: FixToolWindow
    + **Text**: Quản lý bán hàng
    + **Font/Size**:14
    + **Size**: 1339, 667
- **Bước 3**: Thêm các control
    + **MenuStrip**: Kéo menustrip từ toolbox vào trong frm_Main
    + **StatusStrip**: Kéo StatusStrip từ toolbox vào trong frm_main
- **Bước 4**: Add thư viện DotNetBar2 từ file DLL theo link sau đó thêm TabControl từ thư viện này vào FrmMain (cách làm giống như Add control thông thường)
<img src="https://github.com/NguyenMinhPhuc/Pro_QuanLyBanHang/blob/main/Pages/Images/Frm_Main.png?raw=true" height="400px" />


[Xem video hướng dẫn tại đây]()

## Thiết kề Form Login cho hệ thống

form login sử dụng để kiểm soát quyền đăng nhập vào hệ thống của ứng dụng. thông thường có 2 cách để thiết kế cách mở form login của hệ thống phần mềm
- **Cách 1**: khi ứng dụng được mở. form login sẽ được mở trước. sau khi kiểm tra đăng nhập sẽ cho phép mở form chính của ứng dụng
- **Cách 2**: Form chính của ứng dụng sẽ mở lên trước. trong sự kiện load của form chính sẽ gọi mở form login theo dạng ShowDialog(). Kiểm tra đăng nhập thành công chỉ cần tắt form login đi (khuyên dùng).

<img src="https://github.com/NguyenMinhPhuc/Pro_QuanLyBanHang/blob/main/Pages/Images/FrmLogin.png?raw=true" height="300px" />

**Chú ý:** Trong hướng dẫn này sẽ sử dụng cách thứ 2


[Xem video hướng dẫn tại đây]()

## Cách mở form theo dạng thông thường



## Cách mở form theo dạng TabControl

