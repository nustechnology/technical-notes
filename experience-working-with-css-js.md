**I. Lời ngỏ.**
*   Là một RoR developer chắc hẳn nhiều bạn fresher (hay thậm chí là exp dev) thì chủ đề Front-end vẫn là muôn thuở trong 'cuộc chiến' 'QC-Dev'. Nhiều issue xảy ra không đáng có, hay thậm chí là không biết tại sao lại có issue này ngay sau khi mình deploy :p
*   Vì vậy bài viết này mình xin chia sẻ một số kinh nghiệm khi làm việc với CSS/Javascript nhằm giúp các bạn giảm thiểu được phần nào số issue đó.

**II. Nội dung.**

**1\. Kinh nghiệm làm việc với scss/css**
*   Sở dĩ tiêu đề là css mà mình lại giới thiệu scss là vì hầu hết các dự án RoR hiện nay đều dùng scss thay cho css. (scss là gì tham khảo tại [đây](https://sass-lang.com/guide/))

*   1.1 Đặt tên file theo màn hình

    Mục đích: dễ dàng tìm và sửa lỗi đúng nơi, đúng chỗ

    Giả sử ta có cây thư mục views như sau:

    ```
      views
        - homepage
          - index.html
        - campaigns
          - show.html
          - new.html
    ```

    thì css được viết trong các files:

    ```
      stylesheets
        - homepage.scss
        - campaign.scss
        - new_campaign.scs
    ```

*   1.2 Quy tắt xếp chồng (Nested Rules)

    Mục đích: chỉ apply css lên các 'child' elements của một parent element nào đó.

    Giả sử bây giờ mình có một cây HTML như sau:

    ```
    <div class="menu-container">
      <ul class='ul-class'>
        <li class='li-class'>Trang chủ</li>
        <li class='li-class'>Blog</li>
        <li class='li-class'>Dịch vụ</li>
      </ul>
    </div>
    ```

    Thì khi viết scss ta sẽ viết như sau:

    ```
    .menu-container {
      .ul-class {
        .li-class {
          color: blue;
        }
      }
    }
    ```

    Hoặc có thể nối một class của parent elm bằng cách dùng dấu `&` vào trước phần tử đó.

    vd đoạn code trên nhưng thêm class `li-class2` cho `li` và có color red thì viết lại như sau:

    ```
    <li class='li-class li-class2'>Blog</li>
    ```

    ```
    .menu-container {
      .ul-class {
        .li-class {
          color: blue;
          &.li-class2 {
            color: red;
          }
        }
      }
    }
    ```

*   1.3 Luôn luôn sử dụng class để modified css

    Tránh trường hợp modified css qua tag element vd như `div`, `p`..như vậy sẽ dẫn đến trường hợp code mình chạy đúng mà ảnh hưởng chỗ khác

*   1.4 Sử dụng biến: $tên_biến

    Khai báo một giá trị mà ta có thể sử dụng nhiều lần để dễ maintain và thống nhất. Thường đặt ở đầu file

    Thường dùng để set giá trị height, border, shadow, color...
    ```
    $background_color_light_dark: '#828282';
    ```

    Sử dụng:

    ```
    .main-container {
      background: $background_color_light_dark;
    }

    .btn-button-submit {
      background: $background_color_light_dark;
    }
    ```

**2\. Kinh nghiệm làm việc với j**

*   1.1 Add `ref` cho element có bind event

    Thêm `ref` attr với prefix `js_`

    Mục đích: để dễ maintain, dev khác biết được element đó được bind event nào đó để giúp trace bug dễ dàng, viết thêm event cho element đúng chỗ

    vd:

    HTML:

    ```
    <button class='show-alert' ref='js-show-alert '>Show alert</button>
    ```

    Js:

    ```
    $('.show-alert').click(function() {
      alert('message');
    })
    ```

*   1.2 Sử dụng `console.log`, `console.trace` để debug hiệu quả

    Thực ra `console` có 3 loại để debug tuỳ theo mục đích log khác nhau:

    1. `console.info()` (default) # log message info
    2. `console.warn()` # log message as warning
    3. `console.error()` # log message as error

    Set style cho log message(color, background...)

    vd:

    <a href="https://ibb.co/fvdt4o"><img src="https://preview.ibb.co/gSDRPo/Screen_Shot_2018_06_25_at_6_14_08_PM.png" alt="Screen_Shot_2018_06_25_at_6_14_08_PM" border="0"></a>

    Log `object` dưới dạng table

    <a href="https://ibb.co/kj3kjo"><img src="https://preview.ibb.co/bNV9AT/Screen_Shot_2018_06_25_at_6_24_44_PM.png" alt="Screen_Shot_2018_06_25_at_6_24_44_PM" border="0"></a>


    `console.trace()`: log ra dấu vết ngăn xếp, hiển thị ra đường dẫn 'cuộc gọi' được thực hiện để đạt đến điểm gọi `console.trace()`

    ```
    var getUser = function() {
      var getUserName = function() {
        console.trace('here')
      }

      getUserName();

    }

    getUser();
    ```

    Output sẽ như sau:

    <a href="https://imgbb.com/"><img src="https://image.ibb.co/eLuBqT/Screen_Shot_2018_06_25_at_6_32_41_PM.png" alt="Screen_Shot_2018_06_25_at_6_32_41_PM" border="0"></a>

**III. Kết.**

Hy vọng với một số chia sẽ trên sẽ giúp các dev có thể code và debug nhanh hơn. Và đừng quên chia sẽ những kinh nghiệm khác của bạn với mọi

