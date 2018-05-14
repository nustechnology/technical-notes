### 1. Docker là gì?

Docker một nền tảng mở dành cho các lập trình viên, quản trị hệ thống dùng để xây dựng, vận chuyển và chạy các ứng dụng. Hay để cho những developer chân đất như chúng ta dễ hiểu thì Docker một công cụ tạo môi trường được "đóng gói" (còn gọi là Container) trên máy tính mà không làm tác động tới môi trường hiện tại của máy.

### 2. Tại sao phải dùng Docker?

- Sử dụng docker sẽ giúp cho tất cả developer được phát triển cùng trên một môi trường giống hệt nhau. Tránh xảy ra vấn đề "cái này chạy ngon trên máy tui"
- Giúp quá trình setup project mới trở nên dễ dàng hơn.
- Giúp triển khai kiến trúc Mircoservices dễ dàng
- Giúp scale ứng dụng một cách linh hoạt
- ...

### 3.  Một số khái niệm
- `Docker Images`: Mỗi khi bạn muốn chạy ứng dụng Docker là thì bạn cần một cái image, cái image này có thể là HĐH Centos hoặc Ubuntu, đã cài sẵn các ứng dụng Ruby hoặc PHP

- `Docker Registry`: Là kho chứa images. Bạn có thể tạo ra các images của mình và tải lên Private hoặc Public Docker Registry mà bạn muốn. Một trong những  Docker Registry nổi tiếng là: https://hub.docker.com/

- `Docker Container`: hoạt động giống như một thư mục (directory), chứa tất cả những thứ cần thiết để một ứng dụng có thể chạy được. Mỗi một docker container được tạo ra từ một docker image.

- `Dockerfile`: là một file chứa tập hợp các lệnh để Docker có thể đọc và thực hiện để đóng gói một image theo yêu cầu người dùng, chúng ta thường làm việc với tập tin này nhất

### 4. Một số lệnh căn bản

- Build image từ Dockerfile: `docker build`
- Tạo một container mới: `docker create`
- Chạy command trong container đang chạy: `docker exec`
- Xem list tất cả các image: `docker images`
- Xem list tất cả các container đang chạy: `docker ps`
- Chạy command trong container mới: `docker run`
- Xoá một container: `docker rm <ID container>`
- Kéo và đẩy lên hub docker: `docker pull <image>:<tag>` và `docker push <image>:<tag>`

Trên đây là những lệnh chúng ta thường dùng, còn để hiểu rõ hơn  các bạn có thể tìm hiểu thêm tại: https://docs.docker.com/
### 5. Cách viết Dockerfile

Trước hết, để viết được một Dockerfile hoàn chỉnh, các bạn cần hiểu được các command của Dockerfile:

- **FROM:** Là base image để chúng ta tiến hành build một image mới. Command này phải được đặt trên cùng của Dockerfile
- **MAINTAINER:** Command này là tùy chọn, có thể có hoặc không. Nó chưa thông tin của người tiến hành xây dựng lên images.
- **RUN:** Sử dụng khi muốn thực thi một command trong quá trình build image
- **COPY:** Copy một file từ host machine tới docker image. Có thể sử dụng URL cho tệp tin cần copy, khi đó docker sẽ tiến hành tải tệp tin đó đến thư mục đích.
- **ENV:** Định nghĩa các biến môi trường
- **CMD:** Sử dụng khi muốn thực thi (execute) một command trong quá trình build một container mới từ docker image
- **ENTRYPOINT:** Định nghĩa những command mặc định, cái mà sẽ được chạy khi container running.
- **EXPOSE <port> [<port>]** Lệnh dùng để chỉ định rằng containner của image sẽ listen trên các port nào đó trong khi chạy
- **WORKDIR:** Định nghĩa directory cho CMD
- **USER:** Đặt user hoặc UID cho container được tạo bởi image
- **VOLUME:** Cho phép truy cập / liên kết thư mục giữa các container và máy chủ (host machine)

Bây giờ, hãy thực hành bằng việc build một image thao tác với các câu lệnh trên. Ví dụ với project Rails 5 đơn giản:

```
FROM ruby:2.5.0

# utility libraries
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs

# for a JS runtime
RUN apt-get install -y nodejs

# Install RubyGems
ENV BUNDLE_JOBS=2
ENV RAILS_ENV=development

WORKDIR /app/
RUN echo 'gem: --no-rdoc --no-ri' >> ~/.gemrc \
 && gem update --system \
 && gem install bundler --force

COPY Gemfile Gemfile
COPY Gemfile.lock Gemfile.lock

RUN bundle install --with development test

EXPOSE 3000
CMD ["bundle", "exec", "rails", "server", "-b", "0.0.0.0"]
```

Sau khi tạo xong Dockerfile mọi người có thể Build image từ Dockerfile đó:

`docker build -t demo_rails .`

![alt text][docker_building]

Chờ một chút và pha ly cà phê uống (nếu may mắn Dockerfile chúng ta viết đúng chạy hết nếu không bạn fix lỗi rồi chạy lại ) chúng ta sẽ có image của dự án mình rồi.

`docker run -v ~/projects/docker_demo:/app -p 3000:3000 demo_rails`
![alt text][docker_builded]

Và chúng ta sẽ có kết quả sau:

![alt text][rails_docker]

### Tạm kết:

Đây là một bài viết và ví dụ về triển khai một ứng dụng với Docker. Mỗi ứng dụng sẽ có những cách tiếp cận khác nhau, hy vọng với bài viết này sẽ giúp bạn Dockerize ứng dụng của bạn.

Bạn đã sẵn sàng để chạy ứng dụng của bạn trong Docker chưa?


[rails_docker]: https://i.imgur.com/VriHunR.png "Rails Docker"
[docker_building]: https://i.imgur.com/16ws3ZC.png "Docker building"
[docker_builded]: https://i.imgur.com/fe4ZX9J.png "Docker builded"