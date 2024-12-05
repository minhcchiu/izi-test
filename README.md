## IZISOFTWARE EXERCIES NODEJS / MONGODB
Dương Văn Tiến
[https://github.com/Tien2003deptrai/test-backend-izi/blob/main/dco.txt](https://github.com/Tien2003deptrai/test-backend-izi/tree/main)

### Sau khi hoàn thành đẩy code lên github và nộp bài vào sheet:
- https://docs.google.com/spreadsheets/d/1vCYwP6SEAjkblESZrII8Ac8cXLnKBSwq0nNN9hNcrBU/edit?gid=0#gid=0
- Gồm: Email/Phone,  Github


## Yêu cầu
Dưới đây là lược đồ cơ sở dữ liệu (CSDL) quản lý điểm sinh viên, gồm các bảng quan hệ sau:

### SinhVien
- **MaSV**: Mã sinh viên (Primary Key, trong MongoDB là `_id`)
- **HoTen**: Họ tên sinh viên
- **GioiTinh**: Giới tính (true: Nam, false: Nữ)
- **NgaySinh**: Ngày sinh
- **MaLop**: Mã lớp (Foreign Key liên kết với bảng `Lop`)
- **Tinh**: Tỉnh

### Lop
- **MaLop**: Mã lớp (Primary Key, trong MongoDB là `_id`)
- **TenLop**: Tên lớp
- **MaKhoa**: Mã khoa (Foreign Key liên kết với bảng `Khoa`)

### Khoa
- **MaKhoa**: Mã khoa (Primary Key, trong MongoDB là `_id`)
- **TenKhoa**: Tên khoa
- **SoCBGD**: Số cán bộ giảng dạy

### MonHoc
- **MaMH**: Mã môn học (Primary Key, trong MongoDB là `_id`)
- **TenMH**: Tên môn học
- **SoTiet**: Số tiết

### KetQua
- **MaSV**: Mã sinh viên (Foreign Key liên kết với bảng `SinhVien`)
- **MaMH**: Mã môn học (Foreign Key liên kết với bảng `MonHoc`)
- **DiemThi**: Điểm thi

---
Dựa vào lược đồ trên viết các API bằng NodeJs sau: 
- Câu 1: Đưa ra thông tin gồm mã sinh viên, họ tên, tên lớp của tất cả sinh viên. (0,5đ) 
- Câu 2: Cho biết tổng số sinh viên của lớp ‘CNTT 01’. (0,5đ) (không phân biệt chữ thường/in hoa)
- Câu 3: Đưa ra danh sách gồm mã số sinh viên, họ tên và ngày sinh của các sinh viên khoa ‘Cong Nghe Thong Tin’. (0,5đ) 
- Câu 4: Cho biết tổng số lớp của khoa ‘Kinh Te’. (0,5đ)   (không phân biệt chữ thường/in hoa)
- Câu 5: Đưa ra mã khoa, tên khoa và số cán bộ giảng dạy của mỗi khoa. (0,5đ) 
- Câu 6: Đưa ra mã môn học, tên môn học có số tiết lớn hơn 50 tiết. (0,5đ) 
- Câu 7: Đưa ra mã sinh viên, họ tên có điểm thi môn ‘triet hoc’ từ 7 điểm trở lên. (0,5đ) [{_id: "", hoTen: "", ...}]  (không phân biệt chữ thường/in hoa)
- Câu 8: Cho biết sinh viên nào có điểm thi cao nhất và sinh viên có điểm thi thấp nhất môn triet hoc’. (0,5đ) (không phân biệt chữ thường/in hoa)
- Câu 9: Cho biết danh sách từng lớp có tổng bao nhiêu sinh viên. (0,5đ) 
- Câu 10: Đưa ra danh sách sinh viên thuộc ‘Da Nang’. (0,5đ)   // không phân biệt chữ thường/in hoa
- Câu 11: Viết api tính điểm trung bình từ kết quả thi của sinh viên dựa vào mã sinh viên. (1đ) 
- Câu 12: Xếp hạng sinh viên dựa theo điểm trung bình Giỏi >= 8.5, Khá >= 7 và  <8.5, Trung bình >=5 và <7,  Yếu <5 (1đ). 
- Câu 13: Tìm sinh viên có tổng điểm cao nhất. (1đ) 
- Câu 14: Tìm lớp có nhiều sinh viên nhất (1đ). 
- Câu 15: Tìm cặp sinh viên có nhiều môn học chung nhất (1đ)

- PV: Trình bày xử lý 1 bài toán đơn giản


*** Handle ****:
- Câu 1: Đưa ra thông tin gồm mã sinh viên, họ tên, mã lớp của tất cả sinh viên. (0,5đ)
```

import Khoa from '../models/khoa.model'
import Lop from '../models/lop.model'
import MonHoc from '../models/mon-hoc.model'
import SinhVien from '../models/sinh-vien.model'
import KetQua from '../models/ket-qua.model'
import { ObjectId } from 'mongodb'



const getCau1V2 = async () => {
  return SinhVien.find().select('_id hoTen maLop').populate('maLop', 'tenLop')
}
```


**Ví dụ Kết Quả**:
---
Câu 1: 
- GET: http://localhost:3000/v1/bai-taps/cau-1
```json
[
    {
        "_id": "64d9b2f9f29a0e6d9bde45b1",
        "hoTen": "Nguyen Van A (CNTT 01)",
        "maLop": {
            "_id": "64d9c2e1f29a0e6d9bde45b1",
            "tenLop": "CNTT 01"
        }
    },
    {
        "_id": "64d9b2f9f29a0e6d9bde45b2",
        "hoTen": "Nguyen Van B (CNTT 01)",
        "maLop": {
            "_id": "64d9c2e1f29a0e6d9bde45b1",
            "tenLop": "CNTT 01"
        }
    },
    {
        "_id": "64d9b2f9f29a0e6d9bde45b3",
        "hoTen": "Le Thi A (CNTT 01)",
        "maLop": {
            "_id": "64d9c2e1f29a0e6d9bde45b1",
            "tenLop": "CNTT 01"
        }
    }
]
```


#### Seed Data
```

const seedData = async () => {
  const data = {
    sinhViens: [
      {
        hoTen: 'Le Thi B (KT 02)',
        gioiTinh: true,
        ngaySinh: '1998-01-17T01:22:16.185Z',
        maLop: '64d9c2e1f29a0e6d9bde45b4',
        tinh: 'Da Nang',
        _id: '64d9b2f9f29a0e6d9bde45bc',
        __v: 0,
        createdAt: '2024-09-05T15:09:36.239Z',
        updatedAt: '2024-09-05T15:09:36.239Z',
      },
    ],
    lops: [
      {
        tenLop: 'khtn 01',
        maKhoa: '64d9c4e1f29a0e6d9bde45b5',
        _id: '64d9c2e1f29a0e6d9bde45c3',
        __v: 0,
        createdAt: '2024-09-05T15:09:36.246Z',
        updatedAt: '2024-09-05T15:09:36.246Z',
      },
    ],
    khoas: [
      {
        tenKhoa: 'cong nghe thong tin',
        soCBGD: 123456789,
        _id: '64d9c4e1f29a0e6d9bde45b1',
        __v: 0,
        createdAt: '2024-09-05T15:09:36.249Z',
        updatedAt: '2024-09-05T15:09:36.249Z',
      },
      {
        tenKhoa: 'su pham',
        soCBGD: 123456780,
        _id: '64d9c4e1f29a0e6d9bde45ba',
        __v: 0,
        createdAt: '2024-09-05T15:09:36.249Z',
        updatedAt: '2024-09-05T15:09:36.249Z',
      },
    ],
    monHocs: [
      {
        tenMH: 'nhap mon',
        soTiet: 65,
        _id: '64d9c6e1f29a0e6d9bde45ba',
        __v: 0,
        createdAt: '2024-09-05T15:09:36.250Z',
        updatedAt: '2024-09-05T15:09:36.250Z',
      },
    ],
    ketQuas: [
      {
        maSV: '64d9b2f9f29a0e6d9bde45bc',
        maMH: '64d9c6e1f29a0e6d9bde45b8',
        diemThi: 5.5,
        _id: '66d9c9b079d45575ba8104a3',
        __v: 0,
        createdAt: '2024-09-05T15:09:36.261Z',
        updatedAt: '2024-09-05T15:09:36.261Z',
      },
      {
        maSV: '64d9b2f9f29a0e6d9bde45bc',
        maMH: '64d9c6e1f29a0e6d9bde45b9',
        diemThi: 3.8,
        _id: '66d9c9b079d45575ba8104a4',
        __v: 0,
        createdAt: '2024-09-05T15:09:36.261Z',
        updatedAt: '2024-09-05T15:09:36.261Z',
      },
      {
        maSV: '64d9b2f9f29a0e6d9bde45bc',
        maMH: '64d9c6e1f29a0e6d9bde45ba',
        diemThi: 3.3,
        _id: '66d9c9b079d45575ba8104a5',
        __v: 0,
        createdAt: '2024-09-05T15:09:36.261Z',
        updatedAt: '2024-09-05T15:09:36.261Z',
      },
    ],
  }

  await KetQua.deleteMany()
  await SinhVien.deleteMany()
  await Lop.deleteMany()
  await MonHoc.deleteMany()
  await Khoa.deleteMany()

  const [sinhviens, lops, monhocs, khoas, ketquas] = await Promise.all([
    SinhVien.insertMany(data.sinhViens),
    Lop.insertMany(data.lops),
    MonHoc.insertMany(data.monHocs),
    Khoa.insertMany(data.khoas),
    KetQua.insertMany(data.ketQuas),
  ])

  return { sinhviens, lops, monhocs, khoas, ketquas }
}
```





### Schema: nodejs(express), mongoose
``` Sinh Vien
const sinhVienSchema = new Schema(
  {
    hoTen: {
      type: String,
      trim: true,
      required: true,
    },
    gioiTinh: {
      type: Boolean,
      trim: true,
      default: true,
    },
    ngaySinh: Date,
    maLop: {
      type: Schema.Types.ObjectId,
      ref: 'Lop',
      required: true,
    },
    tinh: {
      type: String,
      trim: true,
    },
  },
  {
    timestamps: true,
  }
)
```

``` Lop
const lopSchema = new Schema(
  {
    tenLop: {
      type: String,
      trim: true,
      lowercase: true,
      required: true,
    },
    maKhoa: {
      type: Schema.Types.ObjectId,
      ref: 'Khoa',
      required: true,
    },
  },
  {
    timestamps: true,
  }
)
```

``` Khoa
const khoaSchema = new Schema(
  {
    tenKhoa: {
      type: String,
      trim: true,
      lowercase: true,
      required: true,
    },
    soCBGD: Number,
  },
  {
    timestamps: true,
  }
)
```

``` Mon Hoc
const monHocSchema = new Schema(
  {
    tenMH: {
      type: String,
      trim: true,
      lowercase: true,
      required: true,
    },
    soTiet: {
      type: Number,
      required: true,
    },
  },
  {
    timestamps: true,
  }
)
```

``` Ket Qua
const ketQuaSchema = new Schema(
  {
    maSV: {
      type: Schema.Types.ObjectId,
      ref: 'SinhVien',
      required: true,
    },
    maMH: {
      type: Schema.Types.ObjectId,
      ref: 'MonHoc',
      required: true,
    },
    diemThi: Number,
  },
  {
    timestamps: true,
  }
)
```

### Data Test

```json
{
    "data": {
        "sinhViens": [
            {
                "hoTen": "Nguyen Van A (CNTT 01)",
                "gioiTinh": true,
                "ngaySinh": "2000-12-14T15:46:54.236Z",
                "maLop": "64d9c2e1f29a0e6d9bde45b1",
                "tinh": "Ha Noi",
                "_id": "64d9b2f9f29a0e6d9bde45b1",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.238Z",
                "updatedAt": "2024-09-05T15:09:36.238Z"
            },
            {
                "hoTen": "Nguyen Van B (CNTT 01)",
                "gioiTinh": true,
                "ngaySinh": "1998-07-02T04:04:42.251Z",
                "maLop": "64d9c2e1f29a0e6d9bde45b1",
                "tinh": "Da Nang",
                "_id": "64d9b2f9f29a0e6d9bde45b2",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.239Z",
                "updatedAt": "2024-09-05T15:09:36.239Z"
            },
            {
                "hoTen": "Le Thi A (CNTT 01)",
                "gioiTinh": true,
                "ngaySinh": "1999-01-12T19:14:42.973Z",
                "maLop": "64d9c2e1f29a0e6d9bde45b1",
                "tinh": "Ha Noi",
                "_id": "64d9b2f9f29a0e6d9bde45b3",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.239Z",
                "updatedAt": "2024-09-05T15:09:36.239Z"
            },
            {
                "hoTen": "Le Thi B (CNTT 01)",
                "gioiTinh": true,
                "ngaySinh": "2003-01-30T01:49:10.040Z",
                "maLop": "64d9c2e1f29a0e6d9bde45b1",
                "tinh": "Da Nang",
                "_id": "64d9b2f9f29a0e6d9bde45b4",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.239Z",
                "updatedAt": "2024-09-05T15:09:36.239Z"
            },
            {
                "hoTen": "Nguyen Van A (KT 01)",
                "gioiTinh": true,
                "ngaySinh": "2003-08-17T03:32:04.568Z",
                "maLop": "64d9c2e1f29a0e6d9bde45b3",
                "tinh": "Ha Noi",
                "_id": "64d9b2f9f29a0e6d9bde45b5",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.239Z",
                "updatedAt": "2024-09-05T15:09:36.239Z"
            },
            {
                "hoTen": "Nguyen Van B (KT 01)",
                "gioiTinh": true,
                "ngaySinh": "1998-02-27T10:07:41.642Z",
                "maLop": "64d9c2e1f29a0e6d9bde45b3",
                "tinh": "Da Nang",
                "_id": "64d9b2f9f29a0e6d9bde45b6",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.239Z",
                "updatedAt": "2024-09-05T15:09:36.239Z"
            },
            {
                "hoTen": "Le Thi A (KT 01)",
                "gioiTinh": true,
                "ngaySinh": "2004-05-04T14:44:16.967Z",
                "maLop": "64d9c2e1f29a0e6d9bde45b3",
                "tinh": "Ha Noi",
                "_id": "64d9b2f9f29a0e6d9bde45b7",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.239Z",
                "updatedAt": "2024-09-05T15:09:36.239Z"
            },
            {
                "hoTen": "Le Thi B (KT 01)",
                "gioiTinh": true,
                "ngaySinh": "2002-08-02T00:33:48.491Z",
                "maLop": "64d9c2e1f29a0e6d9bde45b3",
                "tinh": "Da Nang",
                "_id": "64d9b2f9f29a0e6d9bde45b8",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.239Z",
                "updatedAt": "2024-09-05T15:09:36.239Z"
            },
            {
                "hoTen": "Nguyen Van A (KT 02)",
                "gioiTinh": true,
                "ngaySinh": "2004-02-19T20:30:00.798Z",
                "maLop": "64d9c2e1f29a0e6d9bde45b4",
                "tinh": "Ha Noi",
                "_id": "64d9b2f9f29a0e6d9bde45b9",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.239Z",
                "updatedAt": "2024-09-05T15:09:36.239Z"
            },
            {
                "hoTen": "Nguyen Van B (KT 02)",
                "gioiTinh": true,
                "ngaySinh": "2000-10-05T05:56:01.526Z",
                "maLop": "64d9c2e1f29a0e6d9bde45b4",
                "tinh": "Da Nang",
                "_id": "64d9b2f9f29a0e6d9bde45ba",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.239Z",
                "updatedAt": "2024-09-05T15:09:36.239Z"
            },
            {
                "hoTen": "Le Thi A (KT 02)",
                "gioiTinh": true,
                "ngaySinh": "1999-11-21T16:27:00.719Z",
                "maLop": "64d9c2e1f29a0e6d9bde45b4",
                "tinh": "Ha Noi",
                "_id": "64d9b2f9f29a0e6d9bde45bb",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.239Z",
                "updatedAt": "2024-09-05T15:09:36.239Z"
            },
            {
                "hoTen": "Le Thi B (KT 02)",
                "gioiTinh": true,
                "ngaySinh": "1998-01-17T01:22:16.185Z",
                "maLop": "64d9c2e1f29a0e6d9bde45b4",
                "tinh": "Da Nang",
                "_id": "64d9b2f9f29a0e6d9bde45bc",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.239Z",
                "updatedAt": "2024-09-05T15:09:36.239Z"
            }
        ],
        "lops": [
            {
                "tenLop": "cntt 01",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b1",
                "_id": "64d9c2e1f29a0e6d9bde45b1",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.245Z",
                "updatedAt": "2024-09-05T15:09:36.245Z"
            },
            {
                "tenLop": "cntt 02",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b1",
                "_id": "64d9c2e1f29a0e6d9bde45b2",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.245Z",
                "updatedAt": "2024-09-05T15:09:36.245Z"
            },
            {
                "tenLop": "cntt 03",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b1",
                "_id": "64d9c2e1f29a0e6d9bde45b3",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.245Z",
                "updatedAt": "2024-09-05T15:09:36.245Z"
            },
            {
                "tenLop": "kt 01",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b2",
                "_id": "64d9c2e1f29a0e6d9bde45b4",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.246Z",
                "updatedAt": "2024-09-05T15:09:36.246Z"
            },
            {
                "tenLop": "kt 02",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b2",
                "_id": "64d9c2e1f29a0e6d9bde45b5",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.246Z",
                "updatedAt": "2024-09-05T15:09:36.246Z"
            },
            {
                "tenLop": "kt 03",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b2",
                "_id": "64d9c2e1f29a0e6d9bde45b6",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.246Z",
                "updatedAt": "2024-09-05T15:09:36.246Z"
            },
            {
                "tenLop": "qtkd 01",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b3",
                "_id": "64d9c2e1f29a0e6d9bde45b7",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.246Z",
                "updatedAt": "2024-09-05T15:09:36.246Z"
            },
            {
                "tenLop": "qtkd 02",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b3",
                "_id": "64d9c2e1f29a0e6d9bde45b8",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.246Z",
                "updatedAt": "2024-09-05T15:09:36.246Z"
            },
            {
                "tenLop": "qtkd 03",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b3",
                "_id": "64d9c2e1f29a0e6d9bde45b9",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.246Z",
                "updatedAt": "2024-09-05T15:09:36.246Z"
            },
            {
                "tenLop": "luat 01",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b4",
                "_id": "64d9c2e1f29a0e6d9bde45c0",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.246Z",
                "updatedAt": "2024-09-05T15:09:36.246Z"
            },
            {
                "tenLop": "luat 02",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b4",
                "_id": "64d9c2e1f29a0e6d9bde45c1",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.246Z",
                "updatedAt": "2024-09-05T15:09:36.246Z"
            },
            {
                "tenLop": "luat 03",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b4",
                "_id": "64d9c2e1f29a0e6d9bde45c2",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.246Z",
                "updatedAt": "2024-09-05T15:09:36.246Z"
            },
            {
                "tenLop": "khtn 01",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b5",
                "_id": "64d9c2e1f29a0e6d9bde45c3",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.246Z",
                "updatedAt": "2024-09-05T15:09:36.246Z"
            },
            {
                "tenLop": "khtn 02",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b5",
                "_id": "64d9c2e1f29a0e6d9bde45c4",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.246Z",
                "updatedAt": "2024-09-05T15:09:36.246Z"
            },
            {
                "tenLop": "khtn 03",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b5",
                "_id": "64d9c2e1f29a0e6d9bde45c5",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.246Z",
                "updatedAt": "2024-09-05T15:09:36.246Z"
            },
            {
                "tenLop": "nn 01",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b6",
                "_id": "64d9c2e1f29a0e6d9bde45c6",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.247Z",
                "updatedAt": "2024-09-05T15:09:36.247Z"
            },
            {
                "tenLop": "nn 02",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b6",
                "_id": "64d9c2e1f29a0e6d9bde45c7",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.247Z",
                "updatedAt": "2024-09-05T15:09:36.247Z"
            },
            {
                "tenLop": "nn 03",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b6",
                "_id": "64d9c2e1f29a0e6d9bde45c8",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.247Z",
                "updatedAt": "2024-09-05T15:09:36.247Z"
            },
            {
                "tenLop": "dt 01",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b7",
                "_id": "64d9c2e1f29a0e6d9bde45c9",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.247Z",
                "updatedAt": "2024-09-05T15:09:36.247Z"
            },
            {
                "tenLop": "dt 02",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b7",
                "_id": "64d9c2e1f29a0e6d9bde45ca",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.247Z",
                "updatedAt": "2024-09-05T15:09:36.247Z"
            },
            {
                "tenLop": "dt 03",
                "maKhoa": "64d9c4e1f29a0e6d9bde45b7",
                "_id": "64d9c2e1f29a0e6d9bde45cb",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.247Z",
                "updatedAt": "2024-09-05T15:09:36.247Z"
            }
        ],
        "khoas": [
            {
                "tenKhoa": "cong nghe thong tin",
                "soCBGD": 123456789,
                "_id": "64d9c4e1f29a0e6d9bde45b1",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.249Z",
                "updatedAt": "2024-09-05T15:09:36.249Z"
            },
            {
                "tenKhoa": "kinh te",
                "soCBGD": 123456788,
                "_id": "64d9c4e1f29a0e6d9bde45b2",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.249Z",
                "updatedAt": "2024-09-05T15:09:36.249Z"
            },
            {
                "tenKhoa": "quan tri kinh doanh",
                "soCBGD": 123456787,
                "_id": "64d9c4e1f29a0e6d9bde45b3",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.249Z",
                "updatedAt": "2024-09-05T15:09:36.249Z"
            },
            {
                "tenKhoa": "luat",
                "soCBGD": 123456786,
                "_id": "64d9c4e1f29a0e6d9bde45b4",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.249Z",
                "updatedAt": "2024-09-05T15:09:36.249Z"
            },
            {
                "tenKhoa": "khoa hoc tu nhien",
                "soCBGD": 123456785,
                "_id": "64d9c4e1f29a0e6d9bde45b5",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.249Z",
                "updatedAt": "2024-09-05T15:09:36.249Z"
            },
            {
                "tenKhoa": "ngoai ngu",
                "soCBGD": 123456784,
                "_id": "64d9c4e1f29a0e6d9bde45b6",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.249Z",
                "updatedAt": "2024-09-05T15:09:36.249Z"
            },
            {
                "tenKhoa": "dien tu",
                "soCBGD": 123456783,
                "_id": "64d9c4e1f29a0e6d9bde45b7",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.249Z",
                "updatedAt": "2024-09-05T15:09:36.249Z"
            },
            {
                "tenKhoa": "co khi",
                "soCBGD": 123456782,
                "_id": "64d9c4e1f29a0e6d9bde45b8",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.249Z",
                "updatedAt": "2024-09-05T15:09:36.249Z"
            },
            {
                "tenKhoa": "kien truc",
                "soCBGD": 123456781,
                "_id": "64d9c4e1f29a0e6d9bde45b9",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.249Z",
                "updatedAt": "2024-09-05T15:09:36.249Z"
            },
            {
                "tenKhoa": "su pham",
                "soCBGD": 123456780,
                "_id": "64d9c4e1f29a0e6d9bde45ba",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.249Z",
                "updatedAt": "2024-09-05T15:09:36.249Z"
            }
        ],
        "monHocs": [
            {
                "tenMH": "ky nang giao tiep",
                "soTiet": 45,
                "_id": "64d9c6e1f29a0e6d9bde45b1",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.250Z",
                "updatedAt": "2024-09-05T15:09:36.250Z"
            },
            {
                "tenMH": "triet hoc",
                "soTiet": 50,
                "_id": "64d9c6e1f29a0e6d9bde45b2",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.250Z",
                "updatedAt": "2024-09-05T15:09:36.250Z"
            },
            {
                "tenMH": "tin hoc van phong",
                "soTiet": 55,
                "_id": "64d9c6e1f29a0e6d9bde45b3",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.250Z",
                "updatedAt": "2024-09-05T15:09:36.250Z"
            },
            {
                "tenMH": "the duc",
                "soTiet": 45,
                "_id": "64d9c6e1f29a0e6d9bde45b4",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.250Z",
                "updatedAt": "2024-09-05T15:09:36.250Z"
            },
            {
                "tenMH": "quoc phong",
                "soTiet": 60,
                "_id": "64d9c6e1f29a0e6d9bde45b5",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.250Z",
                "updatedAt": "2024-09-05T15:09:36.250Z"
            },
            {
                "tenMH": "thuc tap",
                "soTiet": 40,
                "_id": "64d9c6e1f29a0e6d9bde45b6",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.250Z",
                "updatedAt": "2024-09-05T15:09:36.250Z"
            },
            {
                "tenMH": "do an",
                "soTiet": 70,
                "_id": "64d9c6e1f29a0e6d9bde45b7",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.250Z",
                "updatedAt": "2024-09-05T15:09:36.250Z"
            },
            {
                "tenMH": "truyen thong",
                "soTiet": 50,
                "_id": "64d9c6e1f29a0e6d9bde45b8",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.250Z",
                "updatedAt": "2024-09-05T15:09:36.250Z"
            },
            {
                "tenMH": "marketing",
                "soTiet": 45,
                "_id": "64d9c6e1f29a0e6d9bde45b9",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.250Z",
                "updatedAt": "2024-09-05T15:09:36.250Z"
            },
            {
                "tenMH": "nhap mon",
                "soTiet": 65,
                "_id": "64d9c6e1f29a0e6d9bde45ba",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.250Z",
                "updatedAt": "2024-09-05T15:09:36.250Z"
            }
        ],
        "ketQuas": [
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b1",
                "maMH": "64d9c6e1f29a0e6d9bde45b1",
                "diemThi": 4.8,
                "_id": "66d9c9b079d45575ba81042e",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.252Z",
                "updatedAt": "2024-09-05T15:09:36.252Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b1",
                "maMH": "64d9c6e1f29a0e6d9bde45b2",
                "diemThi": 7.6,
                "_id": "66d9c9b079d45575ba81042f",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.252Z",
                "updatedAt": "2024-09-05T15:09:36.252Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b1",
                "maMH": "64d9c6e1f29a0e6d9bde45b3",
                "diemThi": 3.2,
                "_id": "66d9c9b079d45575ba810430",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.252Z",
                "updatedAt": "2024-09-05T15:09:36.252Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b1",
                "maMH": "64d9c6e1f29a0e6d9bde45b4",
                "diemThi": 6,
                "_id": "66d9c9b079d45575ba810431",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.252Z",
                "updatedAt": "2024-09-05T15:09:36.252Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b1",
                "maMH": "64d9c6e1f29a0e6d9bde45b5",
                "diemThi": 6.6,
                "_id": "66d9c9b079d45575ba810432",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.252Z",
                "updatedAt": "2024-09-05T15:09:36.252Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b1",
                "maMH": "64d9c6e1f29a0e6d9bde45b6",
                "diemThi": 10,
                "_id": "66d9c9b079d45575ba810433",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.252Z",
                "updatedAt": "2024-09-05T15:09:36.252Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b1",
                "maMH": "64d9c6e1f29a0e6d9bde45b7",
                "diemThi": 6.1,
                "_id": "66d9c9b079d45575ba810434",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.252Z",
                "updatedAt": "2024-09-05T15:09:36.252Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b1",
                "maMH": "64d9c6e1f29a0e6d9bde45b8",
                "diemThi": 6.3,
                "_id": "66d9c9b079d45575ba810435",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.252Z",
                "updatedAt": "2024-09-05T15:09:36.252Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b1",
                "maMH": "64d9c6e1f29a0e6d9bde45b9",
                "diemThi": 9.9,
                "_id": "66d9c9b079d45575ba810436",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.252Z",
                "updatedAt": "2024-09-05T15:09:36.252Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b1",
                "maMH": "64d9c6e1f29a0e6d9bde45ba",
                "diemThi": 5.2,
                "_id": "66d9c9b079d45575ba810437",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.252Z",
                "updatedAt": "2024-09-05T15:09:36.252Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b2",
                "maMH": "64d9c6e1f29a0e6d9bde45b1",
                "diemThi": 8.3,
                "_id": "66d9c9b079d45575ba810438",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.253Z",
                "updatedAt": "2024-09-05T15:09:36.253Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b2",
                "maMH": "64d9c6e1f29a0e6d9bde45b2",
                "diemThi": 5.2,
                "_id": "66d9c9b079d45575ba810439",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.253Z",
                "updatedAt": "2024-09-05T15:09:36.253Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b2",
                "maMH": "64d9c6e1f29a0e6d9bde45b3",
                "diemThi": 8,
                "_id": "66d9c9b079d45575ba81043a",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.253Z",
                "updatedAt": "2024-09-05T15:09:36.253Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b2",
                "maMH": "64d9c6e1f29a0e6d9bde45b4",
                "diemThi": 5,
                "_id": "66d9c9b079d45575ba81043b",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.253Z",
                "updatedAt": "2024-09-05T15:09:36.253Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b2",
                "maMH": "64d9c6e1f29a0e6d9bde45b5",
                "diemThi": 9.8,
                "_id": "66d9c9b079d45575ba81043c",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.253Z",
                "updatedAt": "2024-09-05T15:09:36.253Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b2",
                "maMH": "64d9c6e1f29a0e6d9bde45b6",
                "diemThi": 4.9,
                "_id": "66d9c9b079d45575ba81043d",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.253Z",
                "updatedAt": "2024-09-05T15:09:36.253Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b2",
                "maMH": "64d9c6e1f29a0e6d9bde45b7",
                "diemThi": 6.1,
                "_id": "66d9c9b079d45575ba81043e",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.253Z",
                "updatedAt": "2024-09-05T15:09:36.253Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b2",
                "maMH": "64d9c6e1f29a0e6d9bde45b8",
                "diemThi": 5.9,
                "_id": "66d9c9b079d45575ba81043f",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.253Z",
                "updatedAt": "2024-09-05T15:09:36.253Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b2",
                "maMH": "64d9c6e1f29a0e6d9bde45b9",
                "diemThi": 9.8,
                "_id": "66d9c9b079d45575ba810440",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.255Z",
                "updatedAt": "2024-09-05T15:09:36.255Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b2",
                "maMH": "64d9c6e1f29a0e6d9bde45ba",
                "diemThi": 3.6,
                "_id": "66d9c9b079d45575ba810441",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.255Z",
                "updatedAt": "2024-09-05T15:09:36.255Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b3",
                "maMH": "64d9c6e1f29a0e6d9bde45b1",
                "diemThi": 3.5,
                "_id": "66d9c9b079d45575ba810442",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.255Z",
                "updatedAt": "2024-09-05T15:09:36.255Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b3",
                "maMH": "64d9c6e1f29a0e6d9bde45b2",
                "diemThi": 6,
                "_id": "66d9c9b079d45575ba810443",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.255Z",
                "updatedAt": "2024-09-05T15:09:36.255Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b3",
                "maMH": "64d9c6e1f29a0e6d9bde45b3",
                "diemThi": 3.6,
                "_id": "66d9c9b079d45575ba810444",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.255Z",
                "updatedAt": "2024-09-05T15:09:36.255Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b3",
                "maMH": "64d9c6e1f29a0e6d9bde45b4",
                "diemThi": 8.4,
                "_id": "66d9c9b079d45575ba810445",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.255Z",
                "updatedAt": "2024-09-05T15:09:36.255Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b3",
                "maMH": "64d9c6e1f29a0e6d9bde45b5",
                "diemThi": 8.5,
                "_id": "66d9c9b079d45575ba810446",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.255Z",
                "updatedAt": "2024-09-05T15:09:36.255Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b3",
                "maMH": "64d9c6e1f29a0e6d9bde45b6",
                "diemThi": 6.4,
                "_id": "66d9c9b079d45575ba810447",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.255Z",
                "updatedAt": "2024-09-05T15:09:36.255Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b3",
                "maMH": "64d9c6e1f29a0e6d9bde45b7",
                "diemThi": 8.9,
                "_id": "66d9c9b079d45575ba810448",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.255Z",
                "updatedAt": "2024-09-05T15:09:36.255Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b3",
                "maMH": "64d9c6e1f29a0e6d9bde45b8",
                "diemThi": 3.2,
                "_id": "66d9c9b079d45575ba810449",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.255Z",
                "updatedAt": "2024-09-05T15:09:36.255Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b3",
                "maMH": "64d9c6e1f29a0e6d9bde45b9",
                "diemThi": 7.1,
                "_id": "66d9c9b079d45575ba81044a",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b3",
                "maMH": "64d9c6e1f29a0e6d9bde45ba",
                "diemThi": 3.5,
                "_id": "66d9c9b079d45575ba81044b",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b4",
                "maMH": "64d9c6e1f29a0e6d9bde45b1",
                "diemThi": 6.7,
                "_id": "66d9c9b079d45575ba81044c",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b4",
                "maMH": "64d9c6e1f29a0e6d9bde45b2",
                "diemThi": 8.5,
                "_id": "66d9c9b079d45575ba81044d",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b4",
                "maMH": "64d9c6e1f29a0e6d9bde45b3",
                "diemThi": 5.5,
                "_id": "66d9c9b079d45575ba81044e",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b4",
                "maMH": "64d9c6e1f29a0e6d9bde45b4",
                "diemThi": 5.3,
                "_id": "66d9c9b079d45575ba81044f",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b4",
                "maMH": "64d9c6e1f29a0e6d9bde45b5",
                "diemThi": 7.8,
                "_id": "66d9c9b079d45575ba810450",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b4",
                "maMH": "64d9c6e1f29a0e6d9bde45b6",
                "diemThi": 3.1,
                "_id": "66d9c9b079d45575ba810451",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b4",
                "maMH": "64d9c6e1f29a0e6d9bde45b7",
                "diemThi": 3.6,
                "_id": "66d9c9b079d45575ba810452",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b4",
                "maMH": "64d9c6e1f29a0e6d9bde45b8",
                "diemThi": 9.1,
                "_id": "66d9c9b079d45575ba810453",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b4",
                "maMH": "64d9c6e1f29a0e6d9bde45b9",
                "diemThi": 3.4,
                "_id": "66d9c9b079d45575ba810454",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b4",
                "maMH": "64d9c6e1f29a0e6d9bde45ba",
                "diemThi": 8.7,
                "_id": "66d9c9b079d45575ba810455",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b5",
                "maMH": "64d9c6e1f29a0e6d9bde45b1",
                "diemThi": 5.4,
                "_id": "66d9c9b079d45575ba810456",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b5",
                "maMH": "64d9c6e1f29a0e6d9bde45b2",
                "diemThi": 9.4,
                "_id": "66d9c9b079d45575ba810457",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b5",
                "maMH": "64d9c6e1f29a0e6d9bde45b3",
                "diemThi": 6.4,
                "_id": "66d9c9b079d45575ba810458",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b5",
                "maMH": "64d9c6e1f29a0e6d9bde45b4",
                "diemThi": 5.6,
                "_id": "66d9c9b079d45575ba810459",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b5",
                "maMH": "64d9c6e1f29a0e6d9bde45b5",
                "diemThi": 3.7,
                "_id": "66d9c9b079d45575ba81045a",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b5",
                "maMH": "64d9c6e1f29a0e6d9bde45b6",
                "diemThi": 7.8,
                "_id": "66d9c9b079d45575ba81045b",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.256Z",
                "updatedAt": "2024-09-05T15:09:36.256Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b5",
                "maMH": "64d9c6e1f29a0e6d9bde45b7",
                "diemThi": 6.2,
                "_id": "66d9c9b079d45575ba81045c",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b5",
                "maMH": "64d9c6e1f29a0e6d9bde45b8",
                "diemThi": 3.4,
                "_id": "66d9c9b079d45575ba81045d",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b5",
                "maMH": "64d9c6e1f29a0e6d9bde45b9",
                "diemThi": 4,
                "_id": "66d9c9b079d45575ba81045e",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b5",
                "maMH": "64d9c6e1f29a0e6d9bde45ba",
                "diemThi": 4.6,
                "_id": "66d9c9b079d45575ba81045f",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b6",
                "maMH": "64d9c6e1f29a0e6d9bde45b1",
                "diemThi": 4.7,
                "_id": "66d9c9b079d45575ba810460",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b6",
                "maMH": "64d9c6e1f29a0e6d9bde45b2",
                "diemThi": 9,
                "_id": "66d9c9b079d45575ba810461",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b6",
                "maMH": "64d9c6e1f29a0e6d9bde45b3",
                "diemThi": 5.8,
                "_id": "66d9c9b079d45575ba810462",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b6",
                "maMH": "64d9c6e1f29a0e6d9bde45b4",
                "diemThi": 5.1,
                "_id": "66d9c9b079d45575ba810463",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b6",
                "maMH": "64d9c6e1f29a0e6d9bde45b5",
                "diemThi": 4.9,
                "_id": "66d9c9b079d45575ba810464",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b6",
                "maMH": "64d9c6e1f29a0e6d9bde45b6",
                "diemThi": 8.5,
                "_id": "66d9c9b079d45575ba810465",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b6",
                "maMH": "64d9c6e1f29a0e6d9bde45b7",
                "diemThi": 4.7,
                "_id": "66d9c9b079d45575ba810466",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b6",
                "maMH": "64d9c6e1f29a0e6d9bde45b8",
                "diemThi": 9.9,
                "_id": "66d9c9b079d45575ba810467",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b6",
                "maMH": "64d9c6e1f29a0e6d9bde45b9",
                "diemThi": 6.3,
                "_id": "66d9c9b079d45575ba810468",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b6",
                "maMH": "64d9c6e1f29a0e6d9bde45ba",
                "diemThi": 6.5,
                "_id": "66d9c9b079d45575ba810469",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b7",
                "maMH": "64d9c6e1f29a0e6d9bde45b1",
                "diemThi": 7.2,
                "_id": "66d9c9b079d45575ba81046a",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b7",
                "maMH": "64d9c6e1f29a0e6d9bde45b2",
                "diemThi": 9.6,
                "_id": "66d9c9b079d45575ba81046b",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b7",
                "maMH": "64d9c6e1f29a0e6d9bde45b3",
                "diemThi": 3.6,
                "_id": "66d9c9b079d45575ba81046c",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.257Z",
                "updatedAt": "2024-09-05T15:09:36.257Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b7",
                "maMH": "64d9c6e1f29a0e6d9bde45b4",
                "diemThi": 8.3,
                "_id": "66d9c9b079d45575ba81046d",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b7",
                "maMH": "64d9c6e1f29a0e6d9bde45b5",
                "diemThi": 4.2,
                "_id": "66d9c9b079d45575ba81046e",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b7",
                "maMH": "64d9c6e1f29a0e6d9bde45b6",
                "diemThi": 8.8,
                "_id": "66d9c9b079d45575ba81046f",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b7",
                "maMH": "64d9c6e1f29a0e6d9bde45b7",
                "diemThi": 4.4,
                "_id": "66d9c9b079d45575ba810470",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b7",
                "maMH": "64d9c6e1f29a0e6d9bde45b8",
                "diemThi": 4.9,
                "_id": "66d9c9b079d45575ba810471",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b7",
                "maMH": "64d9c6e1f29a0e6d9bde45b9",
                "diemThi": 7.8,
                "_id": "66d9c9b079d45575ba810472",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b7",
                "maMH": "64d9c6e1f29a0e6d9bde45ba",
                "diemThi": 7.1,
                "_id": "66d9c9b079d45575ba810473",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b8",
                "maMH": "64d9c6e1f29a0e6d9bde45b1",
                "diemThi": 8.2,
                "_id": "66d9c9b079d45575ba810474",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b8",
                "maMH": "64d9c6e1f29a0e6d9bde45b2",
                "diemThi": 6.4,
                "_id": "66d9c9b079d45575ba810475",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b8",
                "maMH": "64d9c6e1f29a0e6d9bde45b3",
                "diemThi": 8.3,
                "_id": "66d9c9b079d45575ba810476",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b8",
                "maMH": "64d9c6e1f29a0e6d9bde45b4",
                "diemThi": 6.2,
                "_id": "66d9c9b079d45575ba810477",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b8",
                "maMH": "64d9c6e1f29a0e6d9bde45b5",
                "diemThi": 4.6,
                "_id": "66d9c9b079d45575ba810478",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b8",
                "maMH": "64d9c6e1f29a0e6d9bde45b6",
                "diemThi": 9.5,
                "_id": "66d9c9b079d45575ba810479",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b8",
                "maMH": "64d9c6e1f29a0e6d9bde45b7",
                "diemThi": 8.3,
                "_id": "66d9c9b079d45575ba81047a",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b8",
                "maMH": "64d9c6e1f29a0e6d9bde45b8",
                "diemThi": 6,
                "_id": "66d9c9b079d45575ba81047b",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b8",
                "maMH": "64d9c6e1f29a0e6d9bde45b9",
                "diemThi": 9.9,
                "_id": "66d9c9b079d45575ba81047c",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.258Z",
                "updatedAt": "2024-09-05T15:09:36.258Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b8",
                "maMH": "64d9c6e1f29a0e6d9bde45ba",
                "diemThi": 5.7,
                "_id": "66d9c9b079d45575ba81047d",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b9",
                "maMH": "64d9c6e1f29a0e6d9bde45b1",
                "diemThi": 7.1,
                "_id": "66d9c9b079d45575ba81047e",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b9",
                "maMH": "64d9c6e1f29a0e6d9bde45b2",
                "diemThi": 5.8,
                "_id": "66d9c9b079d45575ba81047f",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b9",
                "maMH": "64d9c6e1f29a0e6d9bde45b3",
                "diemThi": 5.9,
                "_id": "66d9c9b079d45575ba810480",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b9",
                "maMH": "64d9c6e1f29a0e6d9bde45b4",
                "diemThi": 9.7,
                "_id": "66d9c9b079d45575ba810481",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b9",
                "maMH": "64d9c6e1f29a0e6d9bde45b5",
                "diemThi": 7.2,
                "_id": "66d9c9b079d45575ba810482",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b9",
                "maMH": "64d9c6e1f29a0e6d9bde45b6",
                "diemThi": 9.8,
                "_id": "66d9c9b079d45575ba810483",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b9",
                "maMH": "64d9c6e1f29a0e6d9bde45b7",
                "diemThi": 8.6,
                "_id": "66d9c9b079d45575ba810484",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b9",
                "maMH": "64d9c6e1f29a0e6d9bde45b8",
                "diemThi": 7.8,
                "_id": "66d9c9b079d45575ba810485",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b9",
                "maMH": "64d9c6e1f29a0e6d9bde45b9",
                "diemThi": 9,
                "_id": "66d9c9b079d45575ba810486",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45b9",
                "maMH": "64d9c6e1f29a0e6d9bde45ba",
                "diemThi": 5.9,
                "_id": "66d9c9b079d45575ba810487",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45ba",
                "maMH": "64d9c6e1f29a0e6d9bde45b1",
                "diemThi": 4,
                "_id": "66d9c9b079d45575ba810488",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45ba",
                "maMH": "64d9c6e1f29a0e6d9bde45b2",
                "diemThi": 5.7,
                "_id": "66d9c9b079d45575ba810489",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45ba",
                "maMH": "64d9c6e1f29a0e6d9bde45b3",
                "diemThi": 6.6,
                "_id": "66d9c9b079d45575ba81048a",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45ba",
                "maMH": "64d9c6e1f29a0e6d9bde45b4",
                "diemThi": 6.9,
                "_id": "66d9c9b079d45575ba81048b",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45ba",
                "maMH": "64d9c6e1f29a0e6d9bde45b5",
                "diemThi": 4.7,
                "_id": "66d9c9b079d45575ba81048c",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45ba",
                "maMH": "64d9c6e1f29a0e6d9bde45b6",
                "diemThi": 7.4,
                "_id": "66d9c9b079d45575ba81048d",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45ba",
                "maMH": "64d9c6e1f29a0e6d9bde45b7",
                "diemThi": 3.7,
                "_id": "66d9c9b079d45575ba81048e",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45ba",
                "maMH": "64d9c6e1f29a0e6d9bde45b8",
                "diemThi": 4,
                "_id": "66d9c9b079d45575ba81048f",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.259Z",
                "updatedAt": "2024-09-05T15:09:36.259Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45ba",
                "maMH": "64d9c6e1f29a0e6d9bde45b9",
                "diemThi": 9.6,
                "_id": "66d9c9b079d45575ba810490",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45ba",
                "maMH": "64d9c6e1f29a0e6d9bde45ba",
                "diemThi": 9.3,
                "_id": "66d9c9b079d45575ba810491",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bb",
                "maMH": "64d9c6e1f29a0e6d9bde45b1",
                "diemThi": 4.5,
                "_id": "66d9c9b079d45575ba810492",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bb",
                "maMH": "64d9c6e1f29a0e6d9bde45b2",
                "diemThi": 4.1,
                "_id": "66d9c9b079d45575ba810493",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bb",
                "maMH": "64d9c6e1f29a0e6d9bde45b3",
                "diemThi": 3.8,
                "_id": "66d9c9b079d45575ba810494",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bb",
                "maMH": "64d9c6e1f29a0e6d9bde45b4",
                "diemThi": 4.5,
                "_id": "66d9c9b079d45575ba810495",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bb",
                "maMH": "64d9c6e1f29a0e6d9bde45b5",
                "diemThi": 3.7,
                "_id": "66d9c9b079d45575ba810496",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bb",
                "maMH": "64d9c6e1f29a0e6d9bde45b6",
                "diemThi": 8.3,
                "_id": "66d9c9b079d45575ba810497",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bb",
                "maMH": "64d9c6e1f29a0e6d9bde45b7",
                "diemThi": 7.5,
                "_id": "66d9c9b079d45575ba810498",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bb",
                "maMH": "64d9c6e1f29a0e6d9bde45b8",
                "diemThi": 5,
                "_id": "66d9c9b079d45575ba810499",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bb",
                "maMH": "64d9c6e1f29a0e6d9bde45b9",
                "diemThi": 8.7,
                "_id": "66d9c9b079d45575ba81049a",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bb",
                "maMH": "64d9c6e1f29a0e6d9bde45ba",
                "diemThi": 5.8,
                "_id": "66d9c9b079d45575ba81049b",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bc",
                "maMH": "64d9c6e1f29a0e6d9bde45b1",
                "diemThi": 5.6,
                "_id": "66d9c9b079d45575ba81049c",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bc",
                "maMH": "64d9c6e1f29a0e6d9bde45b2",
                "diemThi": 4.6,
                "_id": "66d9c9b079d45575ba81049d",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bc",
                "maMH": "64d9c6e1f29a0e6d9bde45b3",
                "diemThi": 8.3,
                "_id": "66d9c9b079d45575ba81049e",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bc",
                "maMH": "64d9c6e1f29a0e6d9bde45b4",
                "diemThi": 3.6,
                "_id": "66d9c9b079d45575ba81049f",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bc",
                "maMH": "64d9c6e1f29a0e6d9bde45b5",
                "diemThi": 3.6,
                "_id": "66d9c9b079d45575ba8104a0",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bc",
                "maMH": "64d9c6e1f29a0e6d9bde45b6",
                "diemThi": 7.9,
                "_id": "66d9c9b079d45575ba8104a1",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bc",
                "maMH": "64d9c6e1f29a0e6d9bde45b7",
                "diemThi": 9.6,
                "_id": "66d9c9b079d45575ba8104a2",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.260Z",
                "updatedAt": "2024-09-05T15:09:36.260Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bc",
                "maMH": "64d9c6e1f29a0e6d9bde45b8",
                "diemThi": 5.5,
                "_id": "66d9c9b079d45575ba8104a3",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.261Z",
                "updatedAt": "2024-09-05T15:09:36.261Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bc",
                "maMH": "64d9c6e1f29a0e6d9bde45b9",
                "diemThi": 3.8,
                "_id": "66d9c9b079d45575ba8104a4",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.261Z",
                "updatedAt": "2024-09-05T15:09:36.261Z"
            },
            {
                "maSV": "64d9b2f9f29a0e6d9bde45bc",
                "maMH": "64d9c6e1f29a0e6d9bde45ba",
                "diemThi": 3.3,
                "_id": "66d9c9b079d45575ba8104a5",
                "__v": 0,
                "createdAt": "2024-09-05T15:09:36.261Z",
                "updatedAt": "2024-09-05T15:09:36.261Z"
            }
        ]
    }
}
```
