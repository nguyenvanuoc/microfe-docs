---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: page
title: Giới thiệu
---
### VNPT Micro Frontend
Clone thử mục Mircofrontend:
> [https://github.com/nguyenvanuoc/micro-frontends-demo](https://github.com/nguyenvanuoc/micro-frontends-demo)

Để clone thư mục github bạn thực hiện lệnh dưới:

```bash
git clone https://github.com/nguyenvanuoc/micro-frontends-demo
```

Khởi chạy dự án

``` bash

# tiến hành cài đặt các gói thư viện 
npm install

# build thư viện shared components

npm run build shared-components


# khởi chạy tất cả microapp đang có trong dự án

npm run start:all

```

#### Micro Frontend

Micro Frontend là một kiến trúc, chia hệ thống thành nhiều MicroApp nhỏ để phát triển độc lập. Các MicroApp này được kết nối và điều khiển thông qua 1 Switching Layer. Với mô hình Micro Service, bộc lộ 1 “nút thắt cổ chai”, đó là khối UI Monolith. Khi hệ thống càng lớn, UI Monolith càng phình to dẫn đến nhiều nhược điểm mới

* Tổng mã nguồn nặng, ảnh hưởng lớn tới UX
* Khó cập nhật và sửa đổi, do mã nguồn liên quan chằng chịt tới nhau
* Khó mở rộng thêm nghiệp vụ mới
* Khó chia thành nhiều team phát triển độc lập, từ đó tốc độ phát triển bị chậm lại
MicroFrontend ra đời để giải quyết các vấn đề trên





Tổ hợp thư viện và công cụ phục vụ triển khai ứng dụng Frontend trên nền tảng ANGULAR. Hướng tới mục tiêu giúp phát triển các ứng dụng frontend nhanh, đơn giản và chính xác

### Cấu trúc thư mục 

![cấu trúc](/images/ct.png)

- **Projects**: chứa các apps và shared component
    - **Apps** : Thư mục chứa các microapps
    - **Shared-components**: Chưa các thư viện dùng chung, các biến, validation, API, Model, Pipe...

- **src**: Chứa app chính cấu hình gọi các microapps, điều hướng, layout trong thư mục này. 
- **wepack.config.js**: Cấu hình cho apps chính gói các microapps ví dụ :
 
```typescript
    remotes: {
        mailbox: "http://localhost:5202/remoteEntry.js",
        calendar: "http://localhost:5203/remoteEntry.js",
        home: "http://localhost:5204/remoteEntry.js",
        example: "http://localhost:5205/remoteEntry.js",
        login: "http://localhost:5206/remoteEntry.js",
    }
```
### Các thành phần

#### - ng-zorro-antd
Bộ 65+ Angular Component đáp ứng hầu hết các nhu cầu phát triển giao diện [Chi tiết](https://ng.ant.design/docs/introduce/en)
#### - @angular-architects/module-federation
Bộ thư viện giúp hỗ trợ cài đặt microfrontend

#### - @angular-builders/custom-webpack
Bộ thư viện giúp hỗ trợ cài đặt microfrontend

#### -  Icons
Bộ font icon tích hợp nhiều icon hỗ trợ bạn một cách nhanh chóng với class

> [https://nguyenvanuoc.github.io/vuesax-icon/](https://nguyenvanuoc.github.io/vuesax-icon/)

> [https://nguyenvanuoc.github.io/font-apicon/](https://nguyenvanuoc.github.io/font-apicon/)

#### - ngx-scrollbar
Sử dụng scrollbar để chỉnh sửa làm đẹp scroll mặc định của trình duyệt

### Tính năng nổi bật

* Thiết kế "hướng enterprise", đơn giản, sạch.
* 60+ Angular Component, có code mẫu giúp phát triển ứng dụng nhanh.
* Phát triển trên TypeScript tương thích với các ứng dụng Angular hiện tại.
* Giao diện nhẹ, mượt mà.
* Hỗ trợ custom theme.
* Tích hợp bộ giải pháp kiến trúc theo hướng MicroFrontend
* Công cụ hỗ trợ giúp giảm thời gian và tăng độ chính xác

### Hỗ trợ

Chúng tôi xin nhận mọi sự đánh giá cũng như nhận xét của người dùng. Với mục đích cải tiến OneUi ngày càng tốt hơn Mọi thông tin liên hệ, vui lòng gửi mail tới [uocnv@vnpt.vn](mailto:uocnv@vnpt.vn) . Chúng tôi sẽ phản hồi sớm nhất có thể

<!-- 
<div class="home">

  <h1 class="page-heading">Posts</h1>

  <ul class="post-list">
    {% for post in site.posts %}
      <li>
        <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>

        <h2>
          <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
        </h2>
      </li>
    {% endfor %}
  </ul>


</div> -->