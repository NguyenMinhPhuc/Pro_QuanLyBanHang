# mục lục
- [Mục tiêu](#mục-tiêu)
- [Nội dung chính](#nội-dung-chính)
- [Xây dụng lớp ConnectionStringManager.cs](#xây-dựng-lớp-connectionstringmanagercs)
- [Xây dựng lơp MyDatabase](#xây-dựng-lớp-mydatabasecs)


# Mục tiêu
Học xong phần này sinh viên có thể: 
- Hiểu về các thành phần của ADO.NET
- Áp dụng ADO.NET để xây dựng tầng DataLayer cho ứng dụng Windows Form Application.
# Nội dung chính
- Xây dựng lớp ConnectionStringManager.cs (Lớp quản lý chuỗi kết nối): Chuỗi kết nối đến SQL Server của ứng dụng sẽ được lưu trữ trong file (text hoặc ini) và sẽ được quản lý bởi những phương thức trong lớp **ConnectionStringManager.cs**.
- Xây dựng lớp  MyDatabase.cs: Lớp này được xây dựng lên bao gồm các thành phần của ADO.NET với mục tiêu kết nối, và thực hiện các thao tác dữ liệu (CRUD) trong ứng dụng. Bằng cách sử dụng các thành phần của ADO.NET
## Xây dựng lớp ConnectionStringManager.cs
```C#
 using System;
using System.Collections.Generic;
using System.Data.SqlClient;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace QuanLyBanHang.DataLayer
{
    public class ConnectionStringManager
    {
        
        //đối tượng quản lý chuỗi kết nối
        SqlConnectionStringBuilder _sqlConnectionStringBuilder;
        public SqlConnectionStringBuilder _SqlConnectionStringBuilder
        {
            get { return _sqlConnectionStringBuilder; }
            set { _sqlConnectionStringBuilder = value; }
        }
       //Constructor
        public ConnectionStringManager()
        {
            _sqlConnectionStringBuilder = new SqlConnectionStringBuilder();
        }
        //Đọc chuoi ket noi tu file ini (text)
        //path: Đường dẫn của file text chứa chuỗi kết nối
        public void ReadConnectionString(string path)
        {
            using (FileStream fs = new FileStream(path, FileMode.Open, FileAccess.Read, FileShare.Read))
            {
                using (StreamReader sr = new StreamReader(fs))
                {
                    string line = string.Empty;
                    while ((line = sr.ReadLine()) != null)
                    {
                        //đọc từng đòng trong file text
                        line = line.Trim();
                        string[] strings = line.Split(new char[] { ':' });
                        switch (strings[0].ToLower().Trim())
                        {
                            case "server":
                                _sqlConnectionStringBuilder.DataSource = strings[1];
                                break;
                            case "database":
                                _sqlConnectionStringBuilder.InitialCatalog = strings[1];
                                break;
                            case "winnt":
                                _sqlConnectionStringBuilder.IntegratedSecurity = Convert.ToBoolean(strings[1]);
                                break;
                            case "uid":
                                _sqlConnectionStringBuilder.UserID = strings[1];
                                break;
                            case "pwd":
                                _sqlConnectionStringBuilder.Password = strings[1];
                                break;
                        }
                    }
                }
            }
        }
        //Phương thức đọc chuỗi kết nối từ file 
        //Một nạp chồng của phương thức đọc chuỗi kết nối
        public SqlConnectionStringBuilder ReadConnectionString(ref string err,string path)
        {
            SqlConnectionStringBuilder _sqlConnectionStringBuilder=new SqlConnectionStringBuilder();
            using (FileStream fs = new FileStream(path, FileMode.Open, FileAccess.Read, FileShare.Read))
            {
                using (StreamReader sr = new StreamReader(fs))
                {
                    string line = string.Empty;
                    while ((line = sr.ReadLine()) != null)
                    {
                        //server:118.69.126.49
                        line = line.Trim();
                        string[] strings = line.Split(new char[] { ':' });
                        switch (strings[0].ToLower().Trim())
                        {
                            case "server":
                                _sqlConnectionStringBuilder.DataSource = strings[1];
                                break;
                            case "database":
                                _sqlConnectionStringBuilder.InitialCatalog = strings[1];
                                break;
                            case "winnt":
                                _sqlConnectionStringBuilder.IntegratedSecurity = Convert.ToBoolean(strings[1]);
                                break;
                            case "uid":
                                _sqlConnectionStringBuilder.UserID = strings[1];
                                break;
                            case "pwd":
                                _sqlConnectionStringBuilder.Password = strings[1];
                                break;
                        }
                    }
                }
            }
            return _sqlConnectionStringBuilder;
        }

        //Ghi chuỗi kết nối vào file ini
        public void WriteConnectionString(string path)
        {
            using (FileStream fs = new FileStream(path, FileMode.CreateNew, FileAccess.Write, FileShare.Write))
            {
                using (StreamWriter sr = new StreamWriter(fs))
                {
                    sr.WriteLine(string.Format("server:{0}", _sqlConnectionStringBuilder.DataSource));
                    sr.WriteLine(string.Format("database:{0}", _sqlConnectionStringBuilder.InitialCatalog));
                    sr.WriteLine(string.Format("winnt:{0}", _sqlConnectionStringBuilder.IntegratedSecurity.ToString()));
                    sr.WriteLine(string.Format("uid:{0}", _sqlConnectionStringBuilder.UserID));
                    sr.WriteLine(string.Format("pwd:{0}", _sqlConnectionStringBuilder.Password));
                }
            }
        }
        public void WriteConnectionString(string path, SqlConnectionStringBuilder _sqlConnectionStringBuilder)
        {
            using (FileStream fs = new FileStream(path, FileMode.Create, FileAccess.Write, FileShare.Write))
            {
                using (StreamWriter sr = new StreamWriter(fs))
                {
                    sr.WriteLine(string.Format("server:{0}", _sqlConnectionStringBuilder.DataSource));
                    sr.WriteLine(string.Format("database:{0}", _sqlConnectionStringBuilder.InitialCatalog));
                    sr.WriteLine(string.Format("winnt:{0}", _sqlConnectionStringBuilder.IntegratedSecurity.ToString()));
                    sr.WriteLine(string.Format("uid:{0}", _sqlConnectionStringBuilder.UserID));
                    sr.WriteLine(string.Format("pwd:{0}", _sqlConnectionStringBuilder.Password));
                }
            }
        }
    }
}

```

**Ghi Chú:**
Cấu trúc file kết nối. File kết nối được lưu trong thư mục Bin/Debug của dự án.
Được lấy được dẫn theo cú pháp: 

```C#
pathConnectFile = string.Format(@"{0}\Connect.ini",Application.StartupPath);
```
```C#
server:118.69.126.49
database:Data_QuanLyBanHang_HocTap2021
winnt:False
uid:user21ct112
pwd:123456789

``` 

## Xây dựng lớp MyDatabase.cs
Lớp này sử dụng các đối tượng của **ADO.NET** để thực hiện kết nối dữ liệu, và thực hiện các thao tác **CRUD** (Create, Read, Update, Delete) với hệ quản trị CSDL SQL Server

Lớp này sẽ được viết một lần duy nhất trong dự án. và được tham chiếu đến project **BusinessLayer**
```C#
using System;
using System.Data;
using System.Collections.Generic;
using System.Data.SqlClient;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace QuanLyBanHang.DataLayer
{
    public class MyDatabase
    {
        //Đối tượng Ado
        SqlConnection conn;
        SqlCommand cmd;
        SqlDataAdapter adapter;
        //Biến lưu thông tin chuỗi kết nối
        ConnectionStringManager connectionStringManager;
        //Biến lưu lỗi
        string err = string.Empty;
        //Constructor
        public MyDatabase(string path)
        {
            connectionStringManager = new ConnectionStringManager();
            //Khởi tạo đối tượng SqlConnection với một chuỗi kết nối đọc từ file connection.ini 
            conn = new SqlConnection(connectionStringManager.ReadConnectionString(ref err,path).ConnectionString);
        }
        //Phương thức kiểm tra kết nối
        public bool CheckConnect(ref string err)
        {
            try
            {
                if (conn.State == ConnectionState.Open)
                {
                    conn.Close();
                }
                conn.Open();
                return true;
            }
            catch (Exception ex)
            {
                err = ex.Message;
                return false;
            }
            finally
            {
                conn.Close();
            }
        }

        //insert, update, delete
        //Phương thức thực hiện thao tác dữ liệu (Insert, Update, Delete)
        //Phương thức gồm các tham số:
        //- err: Biến tham chiếu dùng để lưu lỗi nếu có
        //- commandeText: Tham số chứa câu truy vấn hoặc tên thủ tục sql
        //- CommandeType: Tham số chỉ định loại của Command
        //- SqlParameter: Tham số lưu danh sách các tham số của thủ tục (store procedure) của Sql 
        // chú ý các phương thức trong lớp này các tham số sẽ giống nhau 
        public int MyExcuteNonQuery(ref string err, string commandText, CommandType commandType, params SqlParameter[] sqlParameters)
        {
            int result = 0;
            try
            {
                //1. step 1 Open connect
                if (conn.State == ConnectionState.Open)
                {
                    conn.Close();
                }
                conn.Open();

                //2. Init Command
                cmd = new SqlCommand(commandText, conn);
                cmd.CommandType = commandType;
                cmd.CommandTimeout = 600;
                if (sqlParameters != null)
                {
                    foreach (SqlParameter param in sqlParameters)
                    {
                        cmd.Parameters.Add(param);
                    }
                }
                //3. Excute Command
                result = cmd.ExecuteNonQuery();
            }
            catch (Exception ex)
            {
                err = ex.Message;
            }
            finally { conn.Close(); }
            return result;
        }

        //Scalar
        //PHương thực thực thi câu truy vấn trả về 1 giá trị duy nhất
        //Giá trị này là một giá trị kiệu Object
        public object MyExcuteScalar(ref string err, string commandText, CommandType commandType, params SqlParameter[] sqlParameters)
        {
            object result = null;
            try
            {
                //1. step 1 Open connect
                if (conn.State == ConnectionState.Open)
                {
                    conn.Close();
                }
                conn.Open();

                //2. Init Command
                cmd = new SqlCommand(commandText, conn);
                cmd.CommandType = commandType;
                cmd.CommandTimeout = 600;
                if (sqlParameters != null)
                {
                    foreach (SqlParameter param in sqlParameters)
                    {
                        cmd.Parameters.Add(param);
                    }
                }
                //3. Excute Command
                result = cmd.ExecuteScalar();
            }
            catch (Exception ex)
            {
                err = ex.Message;
            }
            finally { conn.Close(); }
            return result;
        }
        //SqlDataReader
        //Phương thức thực thi câu lệnh truy vấn trả về một đối tượng SqlDataReader
        public SqlDataReader MyExcuteReader(ref string err, string commandText, CommandType commandType, params SqlParameter[] sqlParameters)
        {
            SqlDataReader result = null;
            try
            {
                //1. step 1 Open connect
                if (conn.State == ConnectionState.Open)
                {
                    conn.Close();
                }
                conn.Open();

                //2. Init Command
                cmd = new SqlCommand(commandText, conn);
                cmd.CommandType = commandType;
                cmd.CommandTimeout = 600;
                if (sqlParameters != null)
                {
                    foreach (SqlParameter param in sqlParameters)
                    {
                        cmd.Parameters.Add(param);
                    }
                }
                //3. Excute Command
                result = cmd.ExecuteReader();
            }
            catch (Exception ex)
            {
                err = ex.Message;
            }

            return result;
        }

        //DataTable
        //Phương thực thực hiện câu lệnh truy vấn và trả về danh sách dữ liệu theo dạng datatable
        public DataTable GetDataTable(ref string err, string commandText, CommandType commandType, params SqlParameter[] sqlParameters)
        {
            DataTable result = null;
            try
            {
                //1. step 1 Open connect
                if (conn.State == ConnectionState.Open)
                {
                    conn.Close();
                }
                conn.Open();

                //2. Init Command
                cmd = new SqlCommand(commandText, conn);
                cmd.CommandType = commandType;
                cmd.CommandTimeout = 600;
                if (sqlParameters != null)
                {
                    foreach (SqlParameter param in sqlParameters)
                    {
                        cmd.Parameters.Add(param);
                    }
                }
                //3. Excute Command
                result = new DataTable();
                adapter = new SqlDataAdapter(cmd);
                adapter.Fill(result);
            }
            catch (Exception ex)
            {
                err = ex.Message;
            }
            finally { conn.Close(); }
            return result;
        }
    }
}

```