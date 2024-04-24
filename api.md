---
layout: page
title: Hướng dẫn kết nối API
permalink: /api/
---

Ví dụ ta có danh sách `API` danh sách, thêm mới, sửa, xóa, tìm kiếm 
```typescript
GET    /products //Lấy danh sách tất cả sản phẩm
GET    /products/{id} // Tìm kiếm sản phẩm theo ID
POST   /products // Thêm mới 1 sản phẩm
PUT    /products/{id} // Sửa 1 sản phẩm
PATCH  /products/{id} // Sửa 1 sản phẩm
DELETE /products/{id} // Xóa 1 sản phẩm
```
Bạn có thể sử dụng  `Postman` để call API
```json
{
    "products": [
        {
            "id": 1,
            "name": "Quần xì nam",
            "cost": 10,
            "quantity": 1000
        },
        {},
        {},
    ]
}
```
Đầu tiên bạn tạo một `product.model.ts` với cấu trúc:
```typescript
export class ProductModel {
    
    constructor(
        public id?: string,
        public name: string,
        public cost: number,
        public quantity: number
    ) { }
}
```

Để viết một hàm trong Angular để gọi một API và nhận tất cả các mục (items) từ API, bạn cần sử dụng Angular HttpClient để thực hiện yêu cầu HTTP. Dưới đây là một ví dụ cơ bản về cách viết một hàm để gọi API và nhận tất cả các mục:

#### Import HttpClient:
Đầu tiên, hãy chắc chắn rằng bạn đã import `HttpClientModule` trong AppModule của bạn và inject `HttpClient` vào service hoặc component của bạn.
```typescript
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class ApiService {
  constructor(private http: HttpClient) {}
}
```
### Viết hàm để gọi API:

Bây giờ, bạn có thể viết một hàm trong service của mình để gọi API và nhận tất cả các mục. với tên `product.services.ts`
```typescript
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';
@Injectable({
  providedIn: 'root'
})
export class ApiService {
    private apiUrl = 'http://localhost:3000/products';

    constructor(private http: HttpClient) {}
    // Lấy tất cả sản phẩm
    getAll(): Observable<any[]> {
        return this.http.get<any[]>(this.apiUrl).pipe(catchError(error => {
            return throwError(error); // Chuyển tiếp lỗi để xử lý ở component
        }));
    }
    // Tìm kiếm sản phẩm theo ID
    find(id: string): Observable<Product[]> {
        return this.http.get<Product[]>(`${this.apiUrl}/${id}`).pipe(
        )
    }
    // Tạo sản phẩm
    create(product: Product): Observable<Product> {

        return this.http.post<Product>(`${this.apiUrl}`, product, {headers}).pipe(catchError(error => {
            return throwError(error); // Chuyển tiếp lỗi để xử lý ở component
        }));
    }
    // Sửa sản phẩm
    update(id: string, product: Product): Observable<Product> {

        return this.http.put<Product>(`${this.apiUrl}/${id}`, product, {headers}).pipe(catchError(error => {
            return throwError(error); // Chuyển tiếp lỗi để xử lý ở component
        }));
    }
    // Xóa sản phẩm
    delete(id: string): Observable<Product> {

        return this.http.delete<Product>(`${this.apiUrl}/${id}`).pipe(catchError(error => {
            return throwError(error); // Chuyển tiếp lỗi để xử lý ở component
        }));
    }
}
```
#### Cấu hình Router
Trong file `app.module.ts` khai báo thế này
```typescript
const routes: Routes = [
  { path: 'products', component: ProductComponent },
  { path: 'products/:id', component: ProductUpdateComponent }, // để lấy ID sửa
  { path: 'products/add-product', component: ProductAddComponent },
]
```
### Lấy danh sách tất cả sản phẩm:

Bây giờ, bạn có thể viết một hàm trong service của mình để gọi API lấy tất cả danh sách sản phẩm

```typescript
import { Component, OnInit } from '@angular/core';
import { ApiService } from './api.service';

@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
  styleUrls: ['./my-component.component.css']
})
export class MyComponent implements OnInit {
  items: any[];

  constructor(private apiService: ApiService) {}

  ngOnInit(): void {
    this.apiService.getAll().subscribe(
      (response) => {
        this.items = response;
        console.log('Items:', this.items);
      },
      (error) => {
        console.error('Error:', error);
      }
    );
  }
}
```


#### Hiển thị kết quả trong template:

Cuối cùng, bạn có thể hiển thị kết quả trong template của component.

```html
<a routerLink="/add-product">Thêm mới</a>
<table>
    <thead>
        <tr>
           <th>Name</th> 
           <th>Cost</th> 
           <th>Quantity</th> 
           <th>Actions</th> 
        </tr>
    <thead>
    <tbody>
        <tr  *ngFor="let item of items">
            <td>{{ item.name }}</td> 
            <td>{{ item.cost }}</td> 
            <td>{{ item.quantity }}</td> 
            <td>
                <a [routerLink]="['/product', item.id]">Sửa</a>
                <a (click)="delete(item.id)" href="javascript:void(0);">Xóa</a>
            </td> 

        </tr>
    </tbody>
</table>
```
Trong ví dụ này, `getAll()` là hàm để gọi API và nhận tất cả các mục từ đường dẫn API đã chỉ định. Kết quả sẽ được trả về dưới dạng một Observable, bạn có thể sử dụng subscribe() để nhận kết quả khi nó có sẵn.

### Viết hàm API tạo sản phẩm


Để tạo một API để tạo mới một mục (item) trong Angular với một model, bạn cần thực hiện các bước sau:

#### Định nghĩa Model:

Bạn sử dụng model `ProductModel` ở trên

#### Sử dụng Service trong Components:

Với API đã khai báo ở trên `create` 


Bây giờ, bạn có thể sử dụng Service đã tạo trong các Angular Components để tạo mới một mục.

```typescript
import { Component } from '@angular/core';
import { ProductService } from './product.services';
import { ProductModel } from './product.model';
import {  FormControl, FormGroup, NonNullableFormBuilder, Validators } from '@angular/forms';
@Component({
  selector: 'app-product-form',
  templateUrl: './product-form.component.html',
  styleUrls: ['./product-form.component.css']
})
export class UserFormComponent {
    product = new Product();
    validateForm: FormGroup<{
        id: FormControl<string>;
        name: FormControl<string>;
        cost: FormControl<number>;
        quantity: FormControl<number>;
    }> = this.fb.group({
        id: ['', null],
        name: ['', Validators.required],
        cost: ['', Validators.required],
        quantity: ['', Validators.required]
    });
    // Check Form Control
    constructor(private productService: ProductService) {}

    onSubmit(): void {
        this.product=this.validateForm.value;
        this.productService.create(product).subscribe(()=>{
            console.log("successfull!!")
        });
    }
}
```

#### Tạo Form để Nhập Dữ liệu:

Trong template HTML của component, tạo một form để nhập dữ liệu của mục mới.
```html
<form [formGroup]="validateForm" (submit)="onSubmit()">
    <input type="text" [(ngModel)]="product.name" formControlName="name" name="name" placeholder="Name">
    <input type="number" [(ngModel)]="product.cost" formControlName="cost" name="cost" placeholder="Cost">
    <input type="number" [(ngModel)]="product.quantity" formControlName="quantity" name="quantity" placeholder="Quantity">
    <button type="submit">Create Product</button>
</form>
```


### Viết hàm API sửa sản phẩm


Để tạo một API để sửa mới một mục (item) trong Angular với một model, bạn cần thực hiện các bước sau:

#### Định nghĩa Model:

Bạn sử dụng model `ProductModel` ở trên

#### Sử dụng Service trong Components:

Với API đã khai báo ở trên `update` 


Bây giờ, bạn có thể sử dụng Service đã tạo trong các Angular Components để tạo mới một mục.

```typescript
import { Component } from '@angular/core';
import { ProductService } from './product.services';
import { ProductModel } from './product.model';
import {  FormControl, FormGroup, NonNullableFormBuilder, Validators } from '@angular/forms';
@Component({
  selector: 'app-product-form',
  templateUrl: './product-form.component.html',
  styleUrls: ['./product-form.component.css']
})
export class UserFormComponent implements OnInit {
    product = new Product();
    id:any; // Gán vào giá trị hoặc lấy từ màn danh sách
    validateForm: FormGroup<{
        id: FormControl<string>;
        name: FormControl<string>;
        cost: FormControl<number>;
        quantity: FormControl<number>;
    }> = this.fb.group({
        id: ['', null],
        name: ['', Validators.required],
        cost: ['', Validators.required],
        quantity: ['', Validators.required]
    });
    // Check Form Control
    constructor(private productService: ProductService) {}
    ngOnInit(): void {
        this.getProductByID(id);
        
        
    }
    onSubmit(): void {
        this.product=this.validateForm.value;
        this.productService.update(id,product).subscribe(()=>{
            console.log("successfull!!")
        });
    }

    // Lấy thông tin sản phẩm theo ID truyền vào form
    getProductByID(id){
        this.productService.find(id).subscribe((res:any)=>{
            this.product=res;
        });
    }
}
```

#### Tạo Form để Nhập Dữ liệu:

Trong template HTML của component, tạo một form để nhập dữ liệu của mục mới.
```html
<form [formGroup]="validateForm" (submit)="onSubmit()">
    <input type="text" [(ngModel)]="product.name" formControlName="name" name="name" placeholder="Name">
    <input type="number" [(ngModel)]="product.cost" formControlName="cost" name="cost" placeholder="Cost">
    <input type="number" [(ngModel)]="product.quantity" formControlName="quantity" name="quantity" placeholder="Quantity">
    <button type="submit">Create Product</button>
</form>
```

### Xóa sản phẩm

Với API đã khai báo ở trên bạn sử dụng  medthod `delete` và viết một hàm `delete` trên file `app.component.ts` của phần danh sách sản phẩm

```typescript
    delete(id){
        this.productService.delete(id).subscribe(()=>{
            this.getAll();
            
        });
    }
```

vậy trong file component ta sẽ được 

```typescript
import { Component, OnInit } from '@angular/core';
import { ProductService } from './product.services';

@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
  styleUrls: ['./my-component.component.css']
})
export class MyComponent implements OnInit {
    items: any[];

    constructor(private productService: ProductService) {}

    ngOnInit(): void {
        this.productService.getAll().subscribe((response) => {
                this.items = response;
                console.log('Items:', this.items);
            },
            (error) => {
                console.error('Error:', error);
            }
        );
    }
    // Xóa sản phẩm theo ID
    delete(id){
        this.productService.delete(id).subscribe(()=>{
            this.getAll();
            
        });
    }
}
```


