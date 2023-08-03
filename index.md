# Index

## 1. Khái niệm

Trong SQL, Index là một đối tượng chứa cặp giá trị key-value trong database được sử dụng để tăng hiệu suất của các câu query trong các bảng có lượng record lớn. Nó giống như chỉ mục của quyển sách vậy, nó giúp database engine nhanh chóng tìm được data cần thiết dựa vào các cột được đánh index.

**TODO**: `Cần giải thích kỹ hơn, key-value cụ thể ở đây chứa giá trị gì. Với index cho multi columns thì được lưu dưới dạng như nào?`
## 2. Một số điểm nổi bật

1. **Mục đích của index**:

Tăng tốc độ truy xuất data, đặc biệt là khi search, sort, hoặc join data giữa các cột với nhau. Nếu không có index, database engine phải thực hiện full table scan để tìm được kêt quả phù hợp, điều đó sẽ làm chậm và tốn tài nguyên, đặc biệt với các table lớn.

2. **Cấu trúc của index**:

Index thường được triển khai theo kiến trúc B-tree. B-tree là kiến trúc phân cấp cân bằng cho phép thực hiện các thao tác search, insert, delete hiệu quả.

**TODO**: `-- Vậy còn có kiến trúc nào khác không? Ưu điểm nhược điểm?`

3. **Cú pháp**:

- Index for single column:

`CREATE INDEX idx_name ON table_name column1;`


- Index for multiple columns:

`CREATE INDEX idx_name ON table_name (column1, column2);`


4. **Các kiểu của index**:

Các database khác nhau sẽ hỗ trợ một vài kiểu index khác nhau, nhưng kiểu phổ biến nhất là index cho một cột. Tuy nhiên, có cả kiểu index tổ hợp, nó bao gồm nhiều columns, cũng đc sử dụng phổ biến.

5. **Ưu điểm của index**

- Tăng hiệu năng cho các truy vấn Select, Join, và Where có liên quan đến cột được đánh index.
- Tốc độ truy xuất data được cải thiện, đặc biệt với các table lớn
- Có thể giúp tối ưu quá trình sorting.

6. **Nhược điểm của index**

- Tăng dung lượng lưu trữ: Các index yêu cầu thêm bộ nhớ, nó có thể là một vấn đề đối với những database lớn.
- Chỉnh sửa data bị chậm: Index cần phải được update mỗi khi data được Insert, Update hoặc Delete. Điều đó có thể khiến các truy vấn bị giảm hiệu năng.
- Chi phí bảo trì: Bảo trì các index thường xuyên có thể được yêu cầu nhằm tối ưu hóa các index
  - Tại sao cần bảo trì Index:
    1. Tối ưu hóa hiệu suất: Khi data trong database thay đổi, các index có thể trở nên rời rạc và kém hiệu quả. Thực hiện bảo trì định kì ví dụ như rebuilding hoặc tổ chức lại các index, giúp tối ưu cấu trúc của chúng và giúp database có hiệu suất cao nhất.
    2. Quản lý các data thay đổi: Khi data được Insert, Update hoặc Delete, index cần được điều chỉnh để phản ánh các thay đổi này. Nếu không bảo trì, các index có thể trở nên outdate
    3. Quản lý bộ nhớ: Các index phân mảnh gây tốn bộ nhớ hơn cần thiết. Bảo trì index giúp lấy lại các bộ nhớ không sử dụng.

7. **Khi nào nên sử dụng index**

- Chỉ đánh index với các cột thường được sử dụng để search, filter hoặc join
- Tránh việc đánh quá nhiều index, sẽ gây giảm hiệu quả truy vấn
- Thường xuyên theo dõi và phân tích hiệu suất của các truy vấn để xác định nhu cầu bổ sung index hoặc tối ưu hóa

