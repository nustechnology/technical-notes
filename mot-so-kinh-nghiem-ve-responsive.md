**I. Lời mở đầu.**

*   Là một full-stack developer, bạn hẳn đã có ít nhiều kinh nghiệm hoặc thậm chí là những bài học đau thương về vấn đề responsive?

*   Bài post này mình sẽ chia sẻ một số kinh nghiệm về vấn đề này. Điều này có thể sẽ giúp các bạn có cái nhìn đúng đắn hơn khi discuss với khách hàng về vấn đề responsive.

**II. Một số chia sẻ.**

**1\. Đừng lệ thuộc vào Bootstrap:**

*   Bootstrap không phải framework duy nhất hỗ trợ responsive. Thậm chí ko cần bất cứ framework nào ta cũng có thể thiết kế giao diện responsive, chỉ cần sử dụng yếu tố cơ bản nhất là @media. Chúng ta không nên tạo một thói quen đến mức trở thành lối mòn là phải có Bootstrap mới làm được responsive.

*   Chúng ta có thể suggest khách hàng để sử dụng Bootstrap nếu project chưa có framework responsive nào. Nhưng nếu đã có thì nên sẵn sàng sử dụng nó.

*   Thực tế là bootstrap phổ biến tới mức nhiều khách hàng muốn tránh sử dụng nó. Phổ biến qúa chưa chắc đã là tốt.

**2\. Quan tâm đến header bar:**

*   Header bar là thành phần biến đổi khá lớn giữa mốc res của desktop và mobile. Ví dụ : <http://jsfiddle.net/giobongio/DwrGK/2/embedded/result/>

*   Header thường chỉ có 2 dạng khác nhau, nằm 2 phía của một breakpoint cụ thể. Độ cao của header trong cùng một dạng cần cố gắng giữ không đổi. Việc thay đổi độ cao của header (trong cùng 1 dạng) có thể ảnh hưởng lớn đến cấu trúc layout. Do vậy cần cần thận khi cố gắng nhét qúa nhiều thông tin vào trong Header. Khi khách hàng đưa yêu cầu, họ chỉ mới nghĩ đến màn hình bự (1920 px chẳng hạn), nên họ nhét rất nhiều thông tin vào header, có khi còn có những đoạn text dài. Tuy nhiên cần chú ý dạng header của màn hình 1920 có thể cùng dạng với màn hình 768 px (nếu dùng Bootstrap). Nếu tại 768 px mà navbar không đủ chỗ hiện đủ thông tin mà cần rớt dòng thì sẽ khá tệ. Do vậy cần lường trước chuyện này xảy ra.

**3\. Làm rõ các mốc responsive:**

*   Việc thiết kế layout Responsive không có nghĩa layout cần hoạt động tốt trên tất cả các cỡ màn hình. Ngay từ bước thiết kế layout ta đã cần xác định rõ các mốc Responsive mà ta cần quan tâm. Các mốc này được gọi là Breakpoint. Cần chú ý là Breakpoint dưới góc nhìn của Dev và của QC có thể khác nhau.

*   Nếu dùng bootstrap, các breakpoint này đã được định nghĩa sẵn, bao gồm: 768px, 992px, 1200px. Các framework khác cũng có bộ breakpoint. Nếu ko dùng framework bạn cũng cần định nghĩa sớm bộ breakpoint.

*   Nếu bộ breakpoint của framework không đủ dùng, bạn cũng có thể định nghĩa thêm một vài mốc. Nhưng việc này cũng cần làm thật sớm, vì nó sẽ ảnh hưởng tới tất cả các page.

*   Đối với góc nhìn của QC, breakpoint chính là cỡ màn hình của một số device thông dụng: <http://www.metaltoad.com/blog/simple-device-diagram-responsive-design-planning>

**4\. Quan tâm đến mốc Breakpoint quan trọng nhất:**

*   Thường thì trong các mốc Breakpoint sẽ có một mốc quan trọng nhất, và thường thì chính là mốc gây ra sự thay đổi về dạng của navbar như đã nói ở trên.

*   Mốc này cần quan tâm nhiều vì cấu trúc layout sẽ thay đổi rất lớn, hơn hẳn các mốc khác. Header và Footer có thể thay đổi hoàn toàn, Sidebar có thể biến mất ...

*   Cần cố gắng để chỉ có một mốc quan trọng nhất như vậy thôi, Ví dụ nếu Navbar thay đổi ở mốc 768px thì cố gắng để Footer cũng thay đổi ở mốc đó luôn. Lý do vì khi thay đổi dạng của header, footer, rất có thể height của 2 thành phần này sẽ thay đổi, và cần xử lý lại layout. Nên nếu chỉ cần xử lý 1 lần thôi thì tốt hơn.

**5\. Zoom?:**

*   Responsive sinh ra với mục đích để cho nhân loại tốt đẹp hơn nói chung, và để cho page hiển thị tốt dưới nhiều màn hình khác nhau nói riêng. Vì vậy cho nên một page responsive cũng không cần có khả năng zoom. Nếu phải zoom mới nhìn rõ một element nào đó, hoặc dễ click hơn, thì rõ ràng page đó responsive chưa tốt. Để disable tính năng zoom của page, ta có thể sử dụng thẻ meta:

        %meta{content: "width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no", name: "viewport"}

**NOTE:** `user-scalable=no` ko còn tác dụng với Safari kể từ iOS 10

*   Khi làm việc với responsive, chúng ta cũng không được test khả năng responsive bằng tính năng zoom của browser. Zoom và responsive là 2 khái niệm rất rất khác nhau. Việc zoom để test responsive là sai hoàn toàn. Ví dụ Chrome có một số agent stylesheet quy định về min của font-size, và con số này khác nhau giữa các mức zoom khác nhau của Chrome.

**6\. Image size:**

*   Ta luôn cần quan tâm dến dung lượng của các resource của page, nhất là đối với các image. Cần cân đối giữa Size và Quality. Đặc biệt là trên mobile, ta cần cân nhắc xem có nên dùng một phiên bản image có size nhỏ hơn so với desktop không. Vì thứ nhất là mobile không cần hình qúa to, không có ích lợi gì (đã tính cả vấn đề về màn hình Retina). Thứ 2 là mobile có tỉ lệ sử dụng 3G khá cao, việc sử dụng hình không phù hợp sẽ ảnh hưởng rất lớn tới thời gian load page.

*   Ta có thể đơn giản là dùng 2 version khác nhau cho image, một cho desktop và 1 cho mobile. Việc này rất đơn giản vì hình chỉ được load khi nó hiện ra (trừ khi gán src vào một object image của JS). Điều này áp dụng được cả cho background-image.

**7\. Màn hình Retina:**

*   Một số dev có hơi lúng túng khi biết cỡ màn hình của Ipad Retina là 1536px x 2048px. Nhưng thực ra Ipad Retina có số Pixel Ratio là 2, nên nói về kích cỡ màn hình dành cho việc responsive thì vẫn là 768x1024 giống Ipad thông thường.

*   Tuy nhiên màn hình Retina có ảnh hưởng đến việc hiển thị Image. Cụ thể là nó có thể hiển thị tốt bức hình có width gốc gấp đôi width được show ra (ratio x 2). Do vậy khi áp dụng ý số 6 cần cân nhắc với màn hình Retina.

*   Nếu cần apply một css riêng cho Retina thì có thể sử dụng đoạn @media sau:

        @media (-webkit-min-device-pixel-ratio: 2) {}

*   Ngoài ra màn hình Retina còn có một điểm đặc biệt nữa, là khả năng chia nhỏ pixel làm đôi. Ví dụ bạn có một text nhỏ, có độ dài lẻ, ví dụ 9px, đặt trong 1 div có độ rộng 12px, và bạn căn text nằm giữa. Thì ở màn hình retina, mỗi bên của text sẽ là 1.5px. Trong khi ở các màn hình bình thường khác thì sẽ là 1 bên 1px, 1 bên 2px.

**III. Kết luận.**

*   Mong rằng một số kinh nghiệm trên sẽ giúp các bạn hiểu rõ hơn về responsive, đặc biệt là khi discuss với khách hàng.
