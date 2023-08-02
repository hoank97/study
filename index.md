# Index

## 1. Khái niệm
Trong SQL, Index là một đối tượng trong database được sử dụng để tăng hiệu suất của các câu query trong các bảng có lượng record lớn. Nó giống như chỉ mục của quyển sách vậy, nó giúp database engine nhanh chóng tìm được data cần thiết dựa vào các cột được đánh index.

## 2. Một số điểm nổi bật

1. **Mục đích của index**: 

Tăng tốc độ truy xuất data, đặc biệt là khi search, sort, hoặc join data giữa các cột với nhau. Nếu không có index, database engine phải thực hiện fulltable scan để tìm được kêt quả phù hợp, điều đó sẽ làm chậm và tốn tài nguyên, đặc biệt với các table lớn

2. **Cấu trúc của index**: 

Index thường được triển khai theo kiến trúc B-tree. B-tree là kiến trúc phân cấp cân bằng cho phép thực hiện các thao tác search, insert, delete hiệu quả.

3. **Cú pháp**:

```CREATE INDEX idx_name ON table_name (column1, column2);```

Tạo 1 index có tên là `idx_name` chứa 2 cột `column1` và `column2` của bảng `table_name`

4. **Các kiểu của index**:

Các database khác nhau sẽ hỗ trợ một vài kiểu index khác nhau, nhưng kiểu phổ biến nhất là index cho một cột. Tuy nhiên, có cả kiểu index tổ hợp, nó bao gồm nhiều columns, cũng đc sử dụng phổ biến.