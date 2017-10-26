**I. Lời mở đầu.**

*   Chắc hẳn bạn đã biết đến Turbolinks phải không? Đó là một plugin vô cùng tuyệt vời, giúp trang web của bạn load nhanh như gióóóóóóóóó luôn.

*   Nó hoạt động bằng cách sử dụng JavaScript để thay đổi nội dung page khi click vào một link bất kì trong trang web (dạng link GET bình thường). Nhờ vậy, nó không cần load lại các JS và CSS resource đã load rồi.

*   Turbolink được mặc định trong Rails 4, nhưng ta cũng có thể dùng nó trong Rails 3 thông qua gem [turbolinks][1]

*   Tuy nhiên bạn có thể thấy rằng trong công ty mình nói riêng thì số project sử dụng nó chưa nhiều. Lý do vì nó gây ra kha khá lỗi bên phía JS. Cho nên việc sử dụng nó cần cân nhắc về chi phí và khả năng áp dụng, tùy theo dự án cụ thể.

*   Trong bài viết này mình sẽ đề cập đến 1 số lỗi hay gặp, một số cách fix, 1 số chú ý khi sử dụng turbolinks, mong là sẽ giúp được mọi người để có thể sử dụng công cụ toẹt vời này.

*   Ở đây mình đã sử dụng turbolinks trong một tình huống có thể nói là khá khó khăn và dễ gặp nhiều bug, sau đó đã chỉnh sửa một số thứ, và hiện giờ đang chạy ok. Cụ thể là rails 3, có dùng theme, có dùng bootstrap, có nhiều JS plugin, trong đó có plugin thấy lỗi rõ rệt khi apply turbolinks, và có cần load js external trong một vài page (không phải tất cả).

**II. Một số issue hay gặp và cách điều trị.**

**NOTE:** Các issue được nhắc đến trong đây đương nhiên là sau khi redirect bằng Turbolinks ít nhất một lần, mình không bàn đến việc lần đầu vào page nhoa :)

**1\. Các plugin JS của bootstrap không hoạt động:**

*   Rất nhiều người từng gặp issue này, vì bootstrap xuất hiện khá thường xuyên trong các project. Bạn sẽ thấy chúng ta ko sử dụng dropdown, modal, bla bla được nữa, hoặc lúc được lúc không.

*   Cách fix: thêm `"data-turbolinks-eval" => false` cho `bootstrap.js` hoặc nguyên cái `application.js`:

        = javascript_include_tag "application_manage", 'data-turbolinks-track' => true, 'data-turbolinks-eval' => false


*   `'data-turbolinks-eval' => false` sẽ giúp đoạn JS đó được execute lại, nhưng không load lại.

*   Nếu vì lý do nào đó bạn không muôn execute lại toàn bộ `application.js`, thì tách `bootstrap.js` ra mà set :)

*   Nếu vì lý do nào đó bạn hoàn toàn ko muốn execute lại, thì có thể bỏ qua cách này, ta vẫn còn cách ultimate phía dưới :)

*   Nếu vì lý do nào đó bạn làm trò này rồi mà vẫn còn bị lỗi (nói boostrap thôi nha), cũng đừng lo, lý do như trên :)

**2\. Một số plugin không hoạt động:**

*   Cái này thì hơi xui, lý do thì trên trời dưới đất lắm, nếu chỉ vì mất events thì còn đơn giản. Bạn có thể thực hiện bước ultimate số 4 trước, trước khi đục vô source code của plugin để tìm lý do.

*   Mình ví dụ một plugin là `jquery.scrollbar`. Nó không hoạt động (hoặc hoạt động sai) mặc dù số event bind cho nó vẫn đầy đủ. Tìm kiếm một hồi thì hóa ra ổng add một cái div ẩn vô body để tính chiều rộng của thanh scroll bar. Mà sau khi body được replace thì mất toi cái div đó rồi.

*   Để fix cái này thì mình đã làm như sau:

        -        if (!browser.data.outer) {
        +        if (!browser.data.outer || !browser.data.outer.is(":visible")) {


**3\. Cần load external JS:**

*   Sẽ có lúc bạn cần load external JS (như google api, filepicker, stripe, ...). Sau khi load xong sẽ có một số config cần set, sau đó mới đến các lệnh JS mà bạn cần init. Vấn đề này coi vậy chứ hóa ra không đơn giản.

*   Cái ta cần ở đây là đảm bảo external JS chỉ load 1 lần, và xác định được thời điểm nó đã load xong để sẵn sàng sử dụng. Và chỉ load external JS khi cần đến nó, ví dụ như JS Stripe rất ít page dùng, mà vào page nào cũng load nó thì khá phí, tuy chỉ là load 1 lần.

*   Để đảm bảo external JS load xong mới chạy lệnh gì đó, các bạn hay dùng `ready function`, nhưng không nên sử dụng nó với turbolinks. Do vậy cũng ko dễ để xác định thời điểm external JS ready.

*   Turbolinks khuyến cáo các js có sử dụng turbolinks cần đặt trên header. Hiện tại theo mình thấy thì `application.js` đặt dưới body vẫn ko sao, nhưng external JS phải đặt trên header mới ko bị load lại. Tuy nhiên muốn SEO tốt thì phải đặt JS dưới body.

*   Tóm lại tổng hợp các vấn đề trên thì cần load external JS bằng `$.getScript` của Jquery theo cách sau đây:

        @loadExternalJs = (exJs, loadedAlready, callback)->
          if loadedAlready()
            callback() if callback
          else
            $.getScript exJs, ->
              callback() if callback
          return

        loadExternalJs '//api.filepicker.io/v1/filepicker.js', (->
          return filepicker?
        ), ->
          filepicker.setKey 'xxx69'
          # more commands
          return


*   `loadedAlreay` là function để kiểm tra xem file JS này đã được load chưa, mỗi JS có thể có dấu hiệu riêng, nếu không có thì bạn có thể check xem đã có node JS này trong DOM chưa :)

*   `callback` là function sẽ gọi ra các nhóm lệnh chỉ được phép chạy sau khi load xong file JS.

*   Với cách này thì bạn có thể linh động load external JS bất cứ khi nào cần, và chỉ khi nào cần.

**4\. Kiểm tra xem có bị thiếu events nào không:**

*   Các bạn có thể dùng đoạn plugin JS sau: [events report][2]

*   Gọi: `$.eventReport()` và `$.eventReport(window)` để theo dõi số lượng và chi tiết các events hiện đang có.

*   Số events này cần được giữ nguyên cho cùng một page. Bạn có thể quan sát số events này để tìm ra các events bị thiếu hoặc bị duplicate, rồi mò đến đúng chỗ để sửa.

*   Ví dụ events bị thiếu là `["document delegates", "click for [data-toggle="dropdown"]", 22]`, từ đây là đoán được của bootstrap rồi. Bạn có thể sửa source code của nó (xóa đi và đưa vào page_init gì đó của riêng bạn, thứ mà sẽ trigger mỗi khi layout được load), hoặc dùng `page:loaded` để bind lại event này.

**5\. Một số chú ý:**

*   Luôn kiểm soát số lượng event, nó cần giống nhau 100% trong cùng 1 page, ko được tăng lên ko được giảm đi.

*   Không bind event lên window hay document khi không thực sự cần thiết. Thay vì thế hãy bind lên body, khi body được replace, các event cũ sẽ tự được xóa. Nếu nhất định phải bind lên window, ví dụ như event window resize, hãy dùng event + namespace, và off nó trước khi on, ví dụ:

        $(window).off("resize.abc").on "resize.abc", -> console.log("haha")


*   Có 1 số ý kiến cho rằng sử dụng `page:loaded` là xử lý được các issue của ready function, song thực tế là ta nên tránh dùng cả 2 thứ thổ tả này.

**III. Kết luận.**

*   Hy vọng sau bài viết này các bạn không còn ác cảm với Turbolinks nữa, mà sẽ sử dụng được những điều kỳ diệu mà nó đem lại.

*   Nếu bạn có kinh nghiệm gì thêm, hãy chia sẻ với mình để mình cập nhật bài viết này.

*   Nếu bạn còn gặp issue gì nữa với Turbolinks, hãy chia sẻ với mình, mình sẽ cố gắng support.

Thanks.

 [1]: https://rubygems.org/gems/turbolinks
 [2]: https://github.com/vietba/events_report/blob/master/events_report.js.coffee