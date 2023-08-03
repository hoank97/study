# Index

## 1. Tổng quan

- Trong SQL, Index là một bảng tra cứu đặc biệt được sử dụng để tăng hiệu suất của các câu query, đặc biệt đối với  các bảng có lượng record lớn. Nếu không có index, database engine phải thực hiện full table scan để tìm được kêt quả phù hợp

- Nó giống như chỉ mục của quyển sách vậy, nó giúp database engine nhanh chóng tìm được data cần thiết dựa vào thông tin của các cột được đánh index.

- Index có thể được tạo cho một hoặc nhiều cột trong database.

- Index được tạo mặc định cho PK và FK.

- Index gồm 2 thành phần chính:
  - `Search key`: chứa bản sao các giá trị của cột được đánh index
  - `Data reference`: chứa con trỏ đến địa chỉ của bản ghi có giá trị cột index tương ứng


## 2. Kiến trúc của index

  1. **B-Tree index**

  ![](https://postgrespro.com/media/2019/05/23/i4_001.png)

  - Dữ liệu index được tổ chức và lưu trữ theo dạng tree: Root, Branch, Leaf

  - Giá trị của các node được tổ chức tăng dần từ trái qua phải.

  - B-Tree index được dùng trong các biểu thức so sánh dạng: BETWEEN, LIKE, =, >, <, … → Tối ưu tốt cho câu lẹnh ORDER BY.

  - Khi truy vấn dữ liệu thì CSDL sẽ không scan dữ liệu trên toàn bộ bảng, việc tìm kiếm trong B-Tree là một quá trình đệ quy. Bắt đầu từ root node → branch → leaf, đến khi tìm được tất cả dữ liệu thỏa mãn với điều kiện thì sẽ dừng lại. 

  2. **Hash index**

  ![](https://d3hi6wehcrq5by.cloudfront.net/itnavi-blog/hU4Tc.png)

  https://viblo.asia/p/hash-index-trong-sql-63vKjwOVZ2R

## 3. Các loại index:

- Clustered index:

  ![](https://images.viblo.asia/f5a1a940-32f7-4856-b8ba-d5c28c071c26.jpeg)

  - Là một kiểu index trong đó địa chỉ của nó trong table trùng với địa chỉ `Search key` (thông thường, clustered index chính là PK). Với clustered index, toàn bộ dữ liệu của row sẽ được lưu ở các `Data reference` thay vì một pointer => Khi tìm kiếm dựa vào clustered index, dữ liệu trả về ngay trên B - tree chính là record cần tìm.
  - Nếu trong 1 table không có PK thì InnoDB sẽ tự lựa chọn theo thứ tự ưu tiên như sau:
    1. Tìm kiếm cột thỏa mãn điều kiện **Unique** và **Not null**
    2. Tự defined ra 1 hidden **Primary key** và **Cluster data** trên cột này.


- Non-clustered index:

  ![](https://images.viblo.asia/9a6e80ee-c2fc-4942-8d9e-760b7edde7b1.png)

  - `Search key`: giá trị của cột được đánh index
  - `Data reference`: trỏ đến vị trí của clustered index
  - Từ đó, flow khi thực hiện query trên 1 bảng dựa trên non-clustered index sẽ là:
    1. Tìm ra record chứa giá trị phù hợp trên B-Tree
    2. Lấy thông tin Data reference trong record đó
    3. Tìm kiếm trong Clustered table
    3. Trả ra result

## 3. **Cú pháp**:

- Index for single column:

`CREATE INDEX idx_name ON table_name column1;`


- Index for multiple columns:

`CREATE INDEX idx_name ON table_name (column1, column2);`

## 5. **Ưu điểm của index**

- Tăng hiệu năng cho các truy vấn Select, Join, và Where có liên quan đến cột được đánh index.
- Tốc độ truy xuất data được cải thiện, đặc biệt với các table lớn
- Có thể giúp tối ưu quá trình sorting.

## 6. **Nhược điểm của index**

- Tăng dung lượng lưu trữ: Các index yêu cầu thêm bộ nhớ, nó có thể là một vấn đề đối với những database lớn.
- Chỉnh sửa data bị chậm: Index cần phải được update mỗi khi data được Insert, Update hoặc Delete. Điều đó có thể khiến các truy vấn bị giảm hiệu năng.
- Chi phí bảo trì: Bảo trì các index thường xuyên có thể được yêu cầu nhằm tối ưu hóa các index
  - Tại sao cần bảo trì Index:
    1. Tối ưu hóa hiệu suất: Khi data trong database thay đổi, các index có thể trở nên rời rạc và kém hiệu quả. Thực hiện bảo trì định kì ví dụ như xóa các index không cần thiết, không hiệu quả, rebuilding hoặc tổ chức lại các index, giúp tối ưu cấu trúc của chúng và giúp database có hiệu suất cao nhất.
    2. Quản lý các data thay đổi: Khi data được Insert, Update hoặc Delete, index cần được điều chỉnh để phản ánh các thay đổi này. Nếu không bảo trì, các index có thể trở nên outdate
    3. Quản lý bộ nhớ: Các index phân mảnh gây tốn bộ nhớ hơn cần thiết. Bảo trì index giúp lấy lại các bộ nhớ không sử dụng.

    -> Khi nào thì index được coi là bị phân mảnh

## 7. **Khi nào nên sử dụng index**

- Chỉ đánh index với các cột thường được sử dụng để search, filter hoặc join
- Tránh việc đánh quá nhiều index, sẽ gây giảm hiệu quả truy vấn
- Thường xuyên theo dõi và phân tích hiệu suất của các truy vấn để xác định nhu cầu bổ sung index hoặc tối ưu hóa

