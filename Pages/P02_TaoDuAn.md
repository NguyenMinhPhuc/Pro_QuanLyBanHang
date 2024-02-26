# Mục lục
[Mục tiêu](#muctieu)


# Mục tiêu

Sau khi học xong phần này sinh viên có thể
- Hiểu Three Layer Architechture
- Áp dụng Three Layer Architechture trong khi triển khai dự án phần mềm
- Tổ chức code trong dự án phù hợp

# Chuẩn bị
## Tạo Cấu trúc dự án
## Chuẩn bị dữ liệu 
  ### Mô hình dữ liệu
  - Users(**UserID**, UserName,Password, DisplayName,...) - bảng người dùng
  - RoleGroups(**RoleGroupID**, GroupName) - Bảng nhóm quyền
  - Permission(**PermissionID**, RoleGroupID, PermissionName, Description, IsDelete) - Bảng phân quyền
  - UserPermisstion()
  ### Thông tin Server
  - Server: 118.69.126.49
  - database: Data_QuanLyBanHang_HocTap2021
  - User: user21ct112
  - password: 123456789
  ### Scrip database
  ```SQL Server
Create database Data_QuanLyBanHang_HocTap2021;
go
Use Data_QuanLyBanHang_HocTap2021;
go
Create Table Users(

)
GO


```
  ### Tạo Cấu trúc Project theo Mô hình 3 layer (Three Layer Architechture)
  - QuanLyBanHang (Blank solution)
    + QuanLyBanHang.DataLayer (Class Library Projects)
    + QuanLyBanHang.BusinessLayer (Class Library Projects)
    + QuanLybanHang.DTO (Class Library Projects)
    + QuanLyBanHang.Common (Class Library Projects)
    + QuanLyBanHang.Presentaition (Windows form Application projects)
   
**Trong đó:**
- **DataLayer**: Project làm việc trực tiếp với Database (SQL Server) chưa các phương thức: Kết nối, CRUD (Create, Read, Update, Delete) sử dụng ADO.NET để thực hiện
- **BusinessLayer**: Project xử lý logic của dự án: làm việc với Lớp giao diện (presentation) và Datalayer. Tầng BusinessLayer tiếp nhận tham số từ giao diện và truyền vào phương thức của DataLayer và nhận về giáo trị từ Database để trả về cho giao điện
- **DTO**: Project chứa những lớp Thực thể (Entity) dùng chuyển tải dữ liệu giữa câc tầng còn lại của dự án
- **Common**: Project chứa những lớp xử lý dùng chung trong dự án.
- **Presintaion**: là một lớp giao diện sử dụng Windows Form Application projects để thiết kế giao diện cho ứng dụng./
