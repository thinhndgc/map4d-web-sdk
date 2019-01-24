# Marker
Lớp Marker cho phép người dùng add một điểm ghim trên bản đồ tại một vị trí xác định.

## 1. Marker & MarkerOptions

```javascript
interface MarkerOptions {
    position: ILatLng //vị trí đặt marker
    visible?: boolean //ẩn hiện marker
    anchor?: IPoint //điểm neo của marker
    icon?: Icon | string //icon marker có thể là 1 icon hoặc 1 url
    elevation?: number //độ cao so với mặt nước biển
    title?: string //tiêu đề của marker
    snippet?: string //mô tả ngắn gọn của marker
    windowAnchor?: IPoint //điểm neo của bảng thông tin marker (bảng thông tin này sẽ hiện khi click vào marker)
  }  
 
  class Marker {
    constructor(options: MarkerOptions) //hàm tạo marker từ 1 MarkerOptions
    //setter
    setMap(map: Map) //khi gọi set map, marker sẽ được hiển thị trên map, khi set map = null, marker sẽ được xóa khỏi map
    setPosition(position: ILatLng) //thay đổi vị trí của marker
    setVisible(visible: boolean) //ẩn hiện marker
    setAnchor(anchor: IPoint) //thay đổi giá trị neo
    setIcon(icon: Icon | string) //thay đổi icon
    setElevation(elevation: number) //thay đổi độ cao
    setTitle(title: string): void //thay đổi tiêu đề 
    setSnippet(snippet: string): void //thay đổi mô tả
    setWindowAnchor(anchor: IPoint): void //thay đổi điểm neo của bảng thông tin marker
    //getter
    getPosition(): LatLng
    isVisible(): boolean
    getAnchor(): Point
    getIcon(): Icon | string
    getElevation(): number
    getMap(): Map
    getTitle(): string    
    getSnippet(): string        
    isInfoWindowShown(): boolean
    hideInfoWindow(): void //ẩn bảng thông tin marker
    showInfoWindow(): void //hiện bảng thông tin marker
  }
  
  class Icon {
    constructor(width: number, height: number, url: string) //tạo icon với các thông số
    clone(): Icon //tạo bản sao 1 icon
    getWidth(): number 
    getHeight(): number
    getUrl(): string
  }
```
***Chú ý:***
  - Width, height của icon và marker mặc định là 32x32 nếu không cung cấp giá trị.
  - Đối với đối tượng icon, hoặc hàm setIcon truyền vào 1 url là đường dẫn tới hình ảnh icon.
    - Để hỗ trợ hiển thị marker trên nhiều màn hình 1x (màn hình thông thường), 2x (màn hình Apple rentina, hoặc các màn hình độ phân giải cao 2k, 4k) bạn phải chuẩn bị 3 ảnh 1x, 2x, 3x (thêm hậu tố @1x, @2x, @3x) và khi gán giá trị cho url cho icon thì truyền vào đường dẫn icon 1x (http://yourwebsite.com/path/image-marker@1x.png). SDK sẽ tự động hiển thị ảnh phù hợp với màn hình đang hiển thị.
    - Nếu đường dẫn hình ảnh truyền vào không có hậu tố @1x thì hình ảnh sẽ được tự động scale để hiển thị phù hợp.

## 2. Tạo marker 

```javascript
//tạo đối tượng marker từ MarkerOption
let marker = new map4d.Marker({
  position: {lat: 10.793113, lng:106.720739},
  icon: new map4d.Icon(32, 32, "https://raw.githubusercontent.com/at-tantv/public_url/master/ic_test_01@1x.png"),
  anchor: [0.5, 1.0],
  title: "Tiêu đề",
  snippet: "Mô tả"
})

//thêm marker vào map    
marker.setMap(map)    
```

## 3. Sự kiện click marker

Phát sinh khi người dùng click vào marker

```javascript
let clickMapsEventClick = this.map.addListener("markerClick", (args) => {
      console.log("Marker clicked: ")
      console.log(args)
    })
    
    //sau khi dùng xong
    clickMapsEventClick.remove();
```

## 4. Tùy chọn hiển thị thông tin marker
Khi marker có tiêu đề hoặc mô tả (title & snippet), nếu người dùng click vào marker, thông tin marker sẽ được hiển thị (dựa vào điểm neo windowAnchor) 

- Tùy biến nội dung thông tin với layout mặc định
```javascript
    map.setInfoContents((marker: map4d.Marker): string => {
       return `<h4>${marker.getTitle()}</h4>` +
        `<p>${marker.getSnippet()}</p>`
    })
```

- Tùy biến bảng hiển thị thông tin bao gồm cả layout và nội dung
```javascript
    map.setInfoWindow((marker: map4d.Marker): string => {
      return `<h4>${marker.getTitle()}</h4>` +
        `<p>${marker.getSnippet()}</p>`
    })
```

License
-------

Copyright (C) 2016 IOT Link Ltd. All Rights Reserved.
