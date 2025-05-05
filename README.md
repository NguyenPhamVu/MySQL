# MySQL
CREATE DATABASE QL_NhanVien;
GO
USE QL_NhanVien;
GO

CREATE TABLE PhongBan (
    MaPhong CHAR(10) PRIMARY KEY,
    TenPhong NVARCHAR(100),
    MoTa NVARCHAR(200),
    NgayThanhLap DATE
);

CREATE TABLE ChucVu (
    MaChucVu CHAR(10) PRIMARY KEY,
    TenChucVu NVARCHAR(100),
    HeSoPhuCap FLOAT
);

CREATE TABLE NhanVien (
    MaNV CHAR(10) PRIMARY KEY,
    HoTen NVARCHAR(100),
    GioiTinh BIT,
    NgaySinh DATE,
    CCCD CHAR(12),
    SoDienThoai VARCHAR(15),
    Email VARCHAR(100),
    DiaChi NVARCHAR(200),
    NgayVaoLam DATE,
    TrangThai NVARCHAR(50),
    MaPhong CHAR(10),
    MaChucVu CHAR(10),
    FOREIGN KEY (MaPhong) REFERENCES PhongBan(MaPhong),
    FOREIGN KEY (MaChucVu) REFERENCES ChucVu(MaChucVu)
);

CREATE TABLE HopDong (
    MaHopDong CHAR(10) PRIMARY KEY,
    MaNV CHAR(10),
    LoaiHopDong NVARCHAR(50),
    NgayBatDau DATE,
    NgayKetThuc DATE,
    LuongCoBan MONEY,
    GhiChu NVARCHAR(200),
    FOREIGN KEY (MaNV) REFERENCES NhanVien(MaNV)
);

CREATE TABLE QuaTrinhCongTac (
    MaQTCongTac INT PRIMARY KEY IDENTITY(1,1),
    MaNV CHAR(10),
    TuNgay DATE,
    DenNgay DATE,
    MaPhong CHAR(10),
    MaChucVu CHAR(10),
    GhiChu NVARCHAR(200),
    FOREIGN KEY (MaNV) REFERENCES NhanVien(MaNV),
    FOREIGN KEY (MaPhong) REFERENCES PhongBan(MaPhong),
    FOREIGN KEY (MaChucVu) REFERENCES ChucVu(MaChucVu)
);

CREATE TABLE TrinhDoHocVan (
    MaTrinhDo CHAR(10) PRIMARY KEY,
    TenTrinhDo NVARCHAR(100)
);

CREATE TABLE NhanVien_TrinhDo (
    MaNV CHAR(10),
    MaTrinhDo CHAR(10),
    ChuyenNganh NVARCHAR(100),
    TruongDaoTao NVARCHAR(150),
    NamTotNghiep INT,
    PRIMARY KEY (MaNV, MaTrinhDo),
    FOREIGN KEY (MaNV) REFERENCES NhanVien(MaNV),
    FOREIGN KEY (MaTrinhDo) REFERENCES TrinhDoHocVan(MaTrinhDo)
);

CREATE TABLE KhenThuong_KyLuat (
    MaQD INT PRIMARY KEY IDENTITY(1,1),
    MaNV CHAR(10),
    LoaiQD NVARCHAR(50),
    NoiDung NVARCHAR(300),
    NgayQD DATE,
    FOREIGN KEY (MaNV) REFERENCES NhanVien(MaNV)
);

CREATE TABLE Luong (
    MaLuong INT PRIMARY KEY IDENTITY(1,1),
    MaNV CHAR(10),
    Thang INT,
    Nam INT,
    LuongCoBan MONEY,
    PhuCap MONEY,
    Thuong MONEY,
    KhauTru MONEY,
    TongLuong AS (LuongCoBan + PhuCap + Thuong - KhauTru) PERSISTED,
    FOREIGN KEY (MaNV) REFERENCES NhanVien(MaNV)
);

CREATE TABLE TaiKhoanHeThong (
    TenDangNhap VARCHAR(50) PRIMARY KEY,
    MatKhau VARCHAR(255),
    MaNV CHAR(10),
    VaiTro NVARCHAR(50),
    FOREIGN KEY (MaNV) REFERENCES NhanVien(MaNV)
);

INSERT INTO PhongBan VALUES 
('PB01', N'Phòng Nhân sự', N'Quản lý nhân sự', '2015-01-01'),
('PB02', N'Phòng Kế toán', N'Xử lý tài chính', '2016-03-15'),
('PB03', N'Phòng Kỹ thuật', N'Phát triển sản phẩm', '2017-06-01');

INSERT INTO ChucVu VALUES
('CV01', N'Giám đốc', 2.5),
('CV02', N'Trưởng phòng', 1.5),
('CV03', N'Nhân viên', 1.0);

INSERT INTO NhanVien VALUES
('NV001', N'Nguyễn Văn A', 1, '1990-05-20', '123456789012', '0912345678', 'a@example.com', N'Hà Nội', '2020-01-15', N'Đang làm', 'PB01', 'CV02'),
('NV002', N'Trần Thị B', 0, '1992-08-10', '234567890123', '0987654321', 'b@example.com', N'HCM', '2021-03-01', N'Đang làm', 'PB02', 'CV03');

INSERT INTO HopDong VALUES
('HD001', 'NV001', N'Không thời hạn', '2020-01-15', NULL, 15000000, N'Chính thức'),
('HD002', 'NV002', N'Thử việc', '2021-03-01', '2021-09-01', 7000000, N'Thử việc 6 tháng');

INSERT INTO QuaTrinhCongTac (MaNV, TuNgay, DenNgay, MaPhong, MaChucVu, GhiChu) VALUES
('NV001', '2020-01-15', NULL, 'PB01', 'CV02', N'Được bổ nhiệm trưởng phòng'),
('NV002', '2021-03-01', NULL, 'PB02', 'CV03', N'Mới gia nhập');

INSERT INTO TrinhDoHocVan VALUES
('TD01', N'Cử nhân'),
('TD02', N'Thạc sĩ');

INSERT INTO NhanVien_TrinhDo VALUES
('NV001', 'TD02', N'Quản trị kinh doanh', N'ĐH Kinh tế Quốc dân', 2013),
('NV002', 'TD01', N'Tài chính - Kế toán', N'ĐH Kinh tế TP.HCM', 2014);

INSERT INTO KhenThuong_KyLuat (MaNV, LoaiQD, NoiDung, NgayQD) VALUES
('NV001', N'Khen thưởng', N'Hoàn thành xuất sắc quý I', '2021-04-01'),
('NV002', N'Kỷ luật', N'Đi trễ nhiều lần', '2022-01-10');

INSERT INTO Luong (MaNV, Thang, Nam, LuongCoBan, PhuCap, Thuong, KhauTru) VALUES
('NV001', 3, 2025, 15000000, 3000000, 2000000, 500000),
('NV002', 3, 2025, 7000000, 1000000, 0, 200000);

INSERT INTO TaiKhoanHeThong VALUES
('admin', '123456', 'NV001', N'Admin'),
('tthib', '123456', 'NV002', N'User');
