### Xử lý vấn đề down-time lúc migration của Rails app

Làm thế nào để deploy ứng dụng Rails ( trong bài này tôi chỉ đề cập tới Rails app, nhưng nó cũng có thể sử dụng cho tất cả mọi ứng dụng web app khác. ) không bị gián đoạn ( down-time )? Với bài viết này mình sẽ giới thiệu một số nguyên nhân gây down-time khi nâng cấp ( migration ) database.

### Những nguyên nhân gây down-time khi nâng cấp database:
  - Việc migration chỉ an toàn nếu trạng thái ứng dụng trước khi deploy có thể hoạt động với những thay đổi mới của Database. Bởi vì sẽ có một khoảng thời gian để quá trình deploy hoàn tất ( trong khi một phần hoặc toàn bộ thay đổi database đã chạy ) trong khi đó ứng dụng cũ vẫn đang được chạy.
  - Việc deploy sẽ không an toàn nếu nó khóa DB quá lâu.

### Các trường hợp gây down-time và cách xử lý:

#### 1. Thêm columns:
 - Sẽ an toàn nếu column được thêm vào là null hoặc số lượng records là nhỏ.
 - Cách xử lý:
  + Bước 1: Thêm columns mới với giá trị là null
  + Bước 2: Set giá trị mặc định bạn muốn cho các records. ( Trong file migration )
  + Bước 3: Change column được thêm vào sang NOT NULL
#### 2. Xóa columns:
  - Không an toàn. Nguyên nhân bởi vì ứng dụng đang sữ dụng vẫn đang sử dụng column cũ.
  - Cách xử lý:
    Tách quá trình deploy thành 3 giai đoạn
    + Giai đoạn 1: Làm cho ứng dụng đang sử dụng tạm thời ignore column đó
      Ví dụ ở đây mình muốn remove column username trong model User:
    ```
      class User < ActiveRecord::base

        # Will remove this code when the column is gone
        def self.columns
          super.reject { |c| c.name == "username" }
        end
      end
    ```
    + Giai đoạn 2: Deploy migration để remove cloumn lên. Lúc này ứng dụng cũ sẽ không sử dụng column cũ nữa ( trong ví dụ trên là column username)
    + Giai đoạn 3: Xóa đoạn code tạm thời ignore column.
#### 3. Sửa tên column:
  - Không an toàn. Nguyên nhân bởi vì ứng dụng đang sử dụng vẫn đang sử dụng column cũ.
  - Cách xử lý:
    Tách quá trình deploy thành 3 giai đoạn:
    + Giai đoạn 1: Thêm column mới, sửa code để ứng dụng ghi giá trị cả columns cũ lẫn columns mới.
    + Giai đoạn 2: Migrate tất cả giá trị từ columns cũ sang columns mới
    + Giai đoạn 3: Xóa columns cũ ra khỏi database
#### 4. Thêm tables: An toàn
#### 5. Xóa tables:
  - Không an toàn. Nguyên nhân bởi vì ứng dụng đang sử dụng vẫn đang sử dụng tables cũ.
  - Cách xử lý:
    - Tương tự với cách xử lý với xóa columns. Tách quá trình deploy thành 2 giai đoạn:
    + Giai đoạn 1: Làm cho ứng dụng hoàn toàn không sử dụng tables đó
    + Giai đoạn 2: Deploy migration để remove tables lên
#### 6. Tạo indexs:
  - Không an toàn. Nguyên nhân bởi vì quá trình tạo indexs sẽ lock database một khoảng thời gian nhất định
  - Cách xử lý:
    + Tùy từng loại database ta có những cách xử lý khác nhau.
    + Với MySQL thì ta có thể sử dụng gem 'lhm'
    + Với PostgreSQL thì có thể sử dụng option concurrently.
#### 7. Xóa indexs:
  - An toàn

*Note:*

Những thông tin trên sẽ đúng với PostgreSQL và MySQL. Tất nhiên nó cũng sẽ đúng với hầu hết với Database engine khác, nhưng đối với với mỗi Database Engine nó sẽ có cách hiện thực cụ thể khác nhau. Với database MySQL ta có thể dùng gem 'lhm' ( https://github.com/soundcloud/lhm ) của SoundCloud để giúp bạn xử lý các trường hợp down-time khi dùng MySQL

### Tổng kết

Sẽ có lúc bạn cần triển khai ứng dụng lên với những thay đổi về database, và việc không gián đoạn ứng dụng và làm ứng dụng mới thích ứng với database structure sẽ là điều rất tuyệt vời. Hy vọng với bài viết trên sẽ giúp các bạn có checklist của mình.
