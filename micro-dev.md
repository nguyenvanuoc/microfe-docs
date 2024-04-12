---
layout: page
title: Các vấn đề của MicroFE
permalink: /microfe-dev/
---


Micro Frontend mang lại nhiều lợi ích trong quá trình phát triển và maintain sản phẩm. Tuy nhiên, vì nguyên nhân chia nhỏ và đặt rải rác các MicroApp tại các khu vực khác nhau, dẫn đến nảy sinh các vấn đề mới cần giải quyết. Ví dụ, vấn đề giao tiếp giữa các microapp, vấn đề tải tài nguyên assets...

### Vấn đề về route

Để chuyển qua lại giữa các thành phần trong 1 hệ thống Micro Frontend, chúng ta sử dụng 3 cách khác nhau

### Nội bộ Micro App

Sử dụng **RouterLink** directive có sẵn của Angular

```html
<a routerLink="link to target">Click here</a>
```

### Sử dụng **href** thông thường

Đôi khi, việc sử dụng 2 cách trên để chuyển hướng router không mang lại hiệu quả như mong đợi, người dùng có thể dùng `href` thông thường để tạo liên kết

```html
<a href="/app2">Link to App2</a>
```

### Vấn đề về assets

Vì vấn đề mỗi Micro App sẽ do 1 đơn vị phát triển độc lập và được upload lên 1 server độc lập. Không thể sửa dụng liên kết tương đối để liên kết tới các assets theo cách thông thường


```html
<img src="asserts/images/a.png">
<script src="asserts/js/abc.js"></script>
...
```

Khi thực chạy, browser sẽ mặc định gọi tới url của domain chính, từ đó dẫn tới sai đường link `404 not found`

Để giải quyết vấn đề này, phải sử dụng các phương pháp sau:

**Sử dụng full path trong các liên kết**

```html
<img src="http://domain.com/asserts/images/a.png">
<script src="http://domain.com/asserts/js/abc.js"></script>
...
```
**Sử dụng base64 data cho image**

```html
<img src="data:image/gif;base64,R0lGODlhEAAQAMQAAORHHOVSKudfOulrSOp3WOyDZu6QdvCchPGolfO0o/XBs/fNwfjZ0frl3/zy7////wAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACH5BAkAABAALAAAAAAQABAAAAVVICSOZGlCQAosJ6mu7fiyZeKqNKToQGDsM8hBADgUXoGAiqhSvp5QAnQKGIgUhwFUYLCVDFCrKUE1lBavAViFIDlTImbKC5Gm2hB0SlBCBMQiB0UjIQA7" alt="star" width="16" height="16">
```
**Sử dụng svg cho image**

```html
<svg ...>
    <ellipse class="ground" .../>
    <path class="kiwi" .../>
</svg>
```

**Sử dụng thông qua pipe**

Trong **Shared-components** đã có sẵn 1 `AssetUrlPipe`

Bạn chỉ việc khai báo __Shared component Module__ vào microapps

```typescript
import { SharedComponentsModule } from 'shared-components';

@NgModule({
    declarations: [
    ],
    imports: [
        SharedComponentsModule
    ],
    providers: [],
    bootstrap: []
})
```

Trong angular template

```html
<img [src]="'image.png' | assetUrl" />
```




