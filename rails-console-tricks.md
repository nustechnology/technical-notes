**I. Lời mở đầu.**

*   Rails console là một tool khá đỗi quen thuộc với dev, chúng ta dùng chúng hằng ngày nhưng vẫn còn khá nhiều thứ chúng ta chưa khám phá hết về chúng.

*  Bài viết này mình sẽ chia sẻ một số kinh nghiệm cũng như tham khảo được, hy vọng sẽ giúp ích một phần nào đó vào công việc của các bạn.

**II. Nội dung**

**1\. Search methods**

Hàm `.methods` sẽ list ra các methods của Object dưới dạng `Array`. Chúng ta có thể search một keywork của method bằng `grep`:

      >> Campaign.first.methods.grep(/name/)
      Campaign Load (0.6ms)  SELECT  "campaigns".* FR...
      =>  [:human_state_name, :state_name]

**2\. Tìm vị trí method được định nghĩa**

Hàm `source_location` sẽ trả về fullpath của file mà định nghĩa method đó:

      >> Campaign.last.method(:name).source_location
      => ["/Users/macbookair/.rvm/gems/../gems/activerecord-5.1.4/lib/active_record/attribute_methods/read.rb", 34]

**3\. Helper Object**

Mặc định rails console sẽ included một object gọi là `helper`. Object này có thể access tới bất kì view hepler nào trong Rails app.

      >> 'ScaleScore'.tableize
      => "scale_scores"

**4\. App object**

Rails console cũng included một object `app`, giống như object cho toàn ứng dụng Rails

Với `app` object bạn có thể access `cookies`, show `_path` helper từ route, hoặc có thể `GET`, `POST`...bạn có thể tìm hiểu kỹ thêm:

     >> app.user_path(1)
     => "/users/1"

     >> app.cookies
     => #<Rack::Test::CookieJar:0x00007fa93c6826c0 @default_host="www.example.com", @cookies=[]>

**5\. Lấy lại kết quả đã execute trước đó**

Trong rails console kết quả vừa thực thi được mặc định gán cho variable `_`, do đó nếu cần xem lại hoặc lấy lại kết quả này để gán cho 1 variable khác thì ta có thể dùng:

     >> _
     => "users/1"

     >> user_path = _
     >> user_path
     => "users/1"

**III. Lời kết.**

*  Hy vọng một số tricks trên sẽ giúp các bạn sử dùng console cho công việc hiệu quả hơn.

*Nguồn: [https://medium.com/@lfv89/rails-console-magic-tricks-da1fdd657d32](https://medium.com/@lfv89/rails-console-magic-tricks-da1fdd657d32)*
