### Xử lý vấn đề downtime lúc migrate database của Rails app

Làm thế nào để deploy ứng dụng Rails (trong bài này tôi chỉ đề cập tới Rails app, nhưng nó cũng có thể sử dụng cho tất cả mọi ứng dụng web app khác.) không bị downtime? Với bài viết này mình sẽ giới thiệu một số nguyên nhân gây downtime khi migration database.

### Những nguyên nhân gây downtime khi nâng cấp database:
  - Việc migrate database chỉ an toàn nếu trạng thái ứng dụng trước khi deploy có thể hoạt động với những thay đổi mới của Database. Bởi vì sẽ có một khoảng thời gian để quá trình deploy hoàn tất (trong khi một phần hoặc toàn bộ thay đổi database đã chạy) trong khi đó ứng dụng cũ vẫn đang được chạy.
  - Việc deploy sẽ không an toàn nếu nó khóa database quá lâu.

### Các trường hợp gây downtime và cách xử lý:

#### 1. Thêm columns:
 - Sẽ an toàn nếu default value của column là null hoặc số lượng records là nhỏ.
 - Cách xử lý:
  + Bước 1: Thêm columns mới với default value là null
  + Bước 2: Set giá trị mặc định bạn muốn cho các records. (Trong file migration)
  + Bước 3: Change column được thêm vào sang NOT NULL
#### 2. Xóa columns:
  - Không an toàn. Nguyên nhân bởi vì ứng dụng vẫn đang sử dụng column cũ.
  - Cách xử lý:
    Tách quá trình deploy thành 3 giai đoạn
    + Giai đoạn 1: Làm cho ứng dụng đang sử dụng tạm thời ignore column đó và loại bỏ những đoạn code đang sử dụng column đó.
      Ví dụ ở đây mình muốn remove column username trong model User:
    ```
      class User < ActiveRecord::base

        # Will remove this code when the column is gone
        def self.columns
          super.reject { |c| c.name == "username" }
        end
      end
    ```
    + Giai đoạn 2: Deploy migration để remove cloumn lên. Lúc này ứng dụng cũ sẽ không sử dụng column cũ nữa (trong ví dụ trên là column username)
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
#### 6. Tạo indexes:
  - Không an toàn. Nguyên nhân bởi vì quá trình tạo indexes sẽ lock database một khoảng thời gian nhất định
  - Cách xử lý:
    + Tùy từng loại database ta có những cách xử lý khác nhau.
    + Với MySQL thì ta có thể sử dụng [gem 'lhm'](https://github.com/soundcloud/lhm).
        * Ý tưởng cơ bản của gem 'lhm' là tạo ra `temporary table` khi thực hiện quá trình migration. Điều này sẽ tránh việc khóa bảng trong quá trình migration.
    + Với PostgreSQL thì có thể sử dụng option [CONCURRENTLY](http://postgresguide.com/performance/indexes.html). 
        * Bình thường khi chúng ta tạo indexes PostgreSQL sẽ lock các query INSERT, UPDATE, DELETE của table đang indexes hiện tại, nếu như table với lượng data rất lớn thì khi tạo indexes sẽ mất rất nhiều thời gian. Khi đó những query CUD đến sẽ không excute được gây ra lỗi. Khi ta thêm option CONCURRENTLY, PostgreSQL sẽ không lock các query CUD nhưng bù lại nó sẽ gây tốn resource nhiều hơn.
#### 7. Xóa indexes:
  - An toàn

*Note:*

Những thông tin trên sẽ đúng với PostgreSQL và MySQL. Tất nhiên nó cũng sẽ đúng với hầu hết với Database engine khác, nhưng đối với với mỗi Database Engine nó sẽ có cách hiện thực cụ thể khác nhau. Với database MySQL ta có thể dùng [gem 'lhm'](https://github.com/soundcloud/lhm) của SoundCloud để giúp bạn xử lý các trường hợp downtime khi dùng MySQL

### Tổng kết

Sẽ có lúc bạn cần triển khai ứng dụng lên với những thay đổi về database, và việc không gián đoạn ứng dụng và làm ứng dụng mới thích ứng với database structure sẽ là điều rất tuyệt vời. Hy vọng với bài viết trên sẽ giúp các bạn có thêm item trong deployment checklist của mình.
