# Cài đặt Map4D-SDK

## 1. Cài đặt

  - Tải mã nguồn mới nhất của SDK từ [link](https://github.com/iotlinkadmin/map4d-web-sdk/tree/master/sdk)

  - Nhúng mã nguồn SDK vào trang web của bạn bằng tag script

```html
<script src="[PATH]/map4dsdk@[version].js"></script>
```

**Chú ý: tên script mã nguồn phải chứa "map4dsdk@", nếu không SDK sẽ không hoạt động.**

## 2. Tạo map

  - Khai báo 1 div để chứa map

```html
<body>
<div id="yourmapid"> <!--Map Container Here--> </div>

<script src="[PATH]/map4dsdk@[version].js"></script>
</body>
```
  - Khởi tạo map với id

```html
<body>
<div id="id"></div>

<script src="[PATH]/map4dsdk@[version].js"></script>

<script>
  let map = new map4d.Map(document.getElementById("yourmapid"),
      {
        center: [10.793113, 106.720739],
        zoom: 17,
        geolocate: true,
        mapControls :true,
        tilt: 0,
        mapControlOptions: map4d.ControlOptions.BOTTOM_RIGHT,
        accessKey: "98fd21346d83bee24dc734231f7609c9"
      }
    )
</script>
</body>
```

- Thiết lập style cho map

```css
  <style>
	body {
	  margin: 0px;
	  height: 100%;
	}

	#yourmapid {
	  position: absolute;
	  width: 100%;
	  height: 100%;
	  background-color: #f2efe9;
	}
  </style>
  ```

## 3. Tùy chọn khi tạo map

```javascript
{
  center: ILatLng,
  zoom?: number,
  tilt?: number,
  bearing?: number,
  minZoom?: number,
  maxZoom?: number,
  geolocate?: boolean,
  accessKey?: string
  mapControls?: boolean,
  mapControlOptions?: ControlOptions
}

enum ControlOptions {
  TOP_LEFT,
  TOP_RIGHT,
  BOTTOM_LEFT,
  BOTTOM_RIGHT
}
```

- center: vị trí khởi tạo trung tâm của map
- zoom, tilt, bearing: mức zoom, độ nghiêng và góc xoay khởi tạo cho map
- minZoom, maxZoom: giới hạn zoom của map (có thể thay đổi bằng hàm setMin/MaxZoom
- geolocate: bật/tắt chức năng lấy vị trí hiện tại của người dùng, mặc định là bật
- accessKey: key để sử dụng map ([đăng ký key](http://map4d.vn)
- mapControls: ẩn hiện bảng điều khiển
- mapControlOptions: vị trí của bảng điều khiển


## 4. Giới hạn mức zoom tối đa và tối thiểu
```javascript
  map.setMinZoom(7)
  map.setMaxZoom(18)
```

## 5. Bật tắt chế độ 3D & 2D
Cho phép tắt bật chế độ 2D và 3D

```javascript
  map.enable3dMode(true) //bật chế độ 3D
  map.enable3dMode(false) //tắt chế độ 3D chuyển về 2D
```

## 6. Chế độ chuyển 2D và 3D
Cho phép thay đổi chế độ chuyển 2D & 3D của map. Có 4 chế độ:

```javascript
enum SwitchMode {
    Auto2DTo3D,
    Auto3DTo2D,
    Auto,
    Manual,
    Default
  }
```

```javascript
  map.setSwitchMode(map4d.SwitchMode.Auto)
```

- Auto2DTo3D:
  - Tự động chuyển chuyển từ chế độ 2D qua 3D khi điều khiển zoom từ mức zoom < 17 lên mức zoom > 17.
  - Khi map đang ở mức zoom > 17, nếu map đang ở chế độ 3D thì khi không cho phép điều khiển zoom xuống mức zoom < 17.
  - Khi map đang ở mức zoom > 17, nếu map đang chế độ 2D, thì map vẫn có thể zoom về mức zoom < 17.
- Auto3DTo2D:
  - **Không** tự động chuyển chuyển từ chế độ 2D qua 3D khi điều khiển zoom từ mức zoom < 17 lên mức zoom > 17.
  - Khi map đang ở mức zoom > 17, map ở chế độ 3D thì khi điều khiển zoom xuống zoom < 17, map sẽ tự động chuyển về chế độ 2D.
- Auto2DTo3D:
  - Tự động chuyển chuyển từ chế độ 2D qua 3D khi điều khiển zoom từ mức zoom < 17 lên mức zoom > 17.
  - Khi map đang ở mức zoom > 17, nếu map đang ở chế độ 3D thì khi không cho phép điều khiển zoom xuống mức zoom < 17.
  - Khi map đang ở mức zoom > 17, nếu map đang chế độ 2D, thì map vẫn có thể zoom về mức zoom < 17.
- Auto:
  - Tự động chuyển chuyển từ chế độ 2D qua 3D khi điều khiển zoom từ mức zoom < 17 lên mức zoom >= 17.
  - Tự động chuyển từ chế độ 3D sang 2D khi điều khiển zoom từ mức zoom >= 17 về mức zoom < 17.
- Manual:
  - Khi map đang ở mức zoom >= 17, nếu map đang ở chế độ 3D thì khi không cho phép điều khiển zoom xuống mức zoom < 17.  
- Default:
  - Chế độ mặc định là **Auto3DTo2D**

**Chú ý: các chế độ này chỉ có tác dụng khi người dùng tương tác với map, không ảnh hưởng khi gọi hàm pan, fly hay setCamera**

## 7. Thay đổi trạng thái và lấy các thông số của map.
Cho phép thay đổi các trạng thái và lấy các thông số của map như độ nghiêng, độ xoay, điểm trung tâm, mức zoom hiện tại

```javascript
setCamera(camera: ICameraPosition): void
getCamera(): CameraPosition

ICameraPosition = CameraPosition | {target: ILatLng, tilt: number, bearing: number, zoom: number}

ILatLng  = LatLng | {lat: number, lng: number} | [number, number]
```

- setCamera:
  - Cho phép thay đổi các thông số camera của map như độ nghiêng, độ xoay, điểm trung tâm, mức zoom.
- getCamera:
  - Cho phép lấy thông tin các thông số camera hiện tại của map.


```javascript
  getMapUrl(): string
```
- getMapUrl: trả về thông tin url hiện tại của map.

```javascript
  getBounds(): LatLngBounds
```
- getBounds: trả về thông tin lat lng bounds của map với các thông số camera hiện tại.

```javascript
  is3DMode(): boolean
```
- is3DMode: trả về thông tin hiện tại map là 2D or 3D.
  - false: 2D mode
  - true:  3D mode

```javascript
  setMinZoom(minZoom: number): void
  getMinZoom(): number
```
- setMinZoom: thiết lập mức zoom tối thiểu của map.
- getMinZoom: trả về thông tin mức zoom tối thiểu của map.

```javascript
  setMaxZoom(maxZoom: number): void
  getMaxZoom(): number
```
- setMaxZoom: thiết lập mức zoom tối đa của map.
- getMaxZoom: trả về thông tin mức zoom tối đa của map.

```javascript
  getPreferMinZoom(): number
```
- getPreferMinZoom: trả về thông tin mức zoom tối thiểu theo trạng thái map(2D or 3D) và theo chế độ chuyển đổi (Auto2DTo3D, Auto3DTo2D, Auto, Manual)
  - **Ví dụ**:

  - Map đang ở chế độ Manual:   
  Nếu map đang ở 2D: trả về mức zoom tối thiếu khi đang ở chế độ 2D
  Nếu map đang ở 3D: trả về mức zoom tối thiểu khi đang ở chế độ 3D (Vì chế độ này không cho phép chuyển từ 3D xuống 2D) => [ minZoom >= 17 ]

  - Map đang ở chế độ Auto:
  Nếu map đang ở chế độ 2D: trả về mức zoom tối thiếu khi đang ở chế độ 2D
  Nếu map đang ở chế độ 2D: trả về mức zoom tối thiếu khi đang ở chế độ 2D

  - Map đang ở chế độ Auto3DTo2D:
  Nếu map đang ở chế độ 2D: trả về mức zoom tối thiếu khi đang ở chế độ 2D
  Nếu map đang ở chế độ 3D: trả về mức zoom tối thiều khi đang ở chế độ 2D (Vì chế độ này cho phép chuyển từ 3D xuống 2D)

  - Map đang ở chế độ Auto2DTo3D:
  Nếu map đang ở chế độ 2D: trả về mức zoom tối thiếu khi đang ở chế độ 2D
  Nếu map đang ở chế độ 3D: trả về mức zoom tối thiều khi đang ở chế độ 3D (Vì chế độ này không cho phép chuyển từ 3D xuống 2D) => [ minZoom >= 17 ]

  == Chú ý: mở mức zoom >= 17 mới có chế độ 3D và tìm hiểu chi tiết về các chế độ ở phía trên ==

## 8. Thay đổi thời gian của map
Map 4D SDK cho phép người dùng thiết lập thời gian cho map, dữ liệu 3D và các địa điểm sẽ được lấy theo thời gian người dùng thiết lập, mặc định sẽ lấy thời gian hiện tại.

```javascript
map.setTime(new Date())
```
## 9. Di chuyển map
Cho phép di chuyển map đến một vị trí bất kỳ

```javascript
    panBy(offset: IPoint, animationOptions?: AnimationOptions): void
    panTo(destination: ILatLng, animationOptions?: AnimationOptions): void
    flyTo(destination: ILatLng, zoom: number, animationOptions?: AnimationOptions): void
```

AnimationOption:

```javascript
  interface AnimationOptions {
    duration?: number
    easing?: (arg0: number) => number
    animate?: boolean
  }
```

IPoint:
```javascript
IPoint  = Point | {lat: number, lng: number} | [number, number]
```

- **panBy**: di chuyển map 1 khoảng x, y tính theo đơn vị point
  - offset: Point | {x: number, y: number} | [number, number]
  - animationOptions: tùy chọn, mặc định là null. Cho phép tùy biến chuyển động.
- **panTo**: di chuyển map đến 1 vị trí (LatLng) bất kỳ
  - destination: LatLng | {lat: number, lng: number} | [number, number]
  - animationOptions: tùy chọn, mặc định là null. Cho phép tùy biến chuyển động.
- **flyTo**: di chuyển map đến 1 vị trí (LatLng) bất kỳ
  - destination: LatLng | {lat: number, lng: number} | [number, number]
  - zoom: giá trị zoom sau khi di chuyển tới vị trí đích
  - animationOptions: tùy chọn, mặc định là null. Cho phép tùy biến chuyển động.  

 ## 10. Ghi chú
 Các kiểu dữ liệu **Point, LatLng, CameraPosition, SwitchMode, ControlOptions** là các kiểu dữ liệu của Map4D-SDK, muốn sử dụng được phải thông qua **module map4d**
 Ví dụ:
 ```javascript
 let location = new map4d.LatLng(10, 108);
 let point = new map4d.Point(1, 2)
 let camera = new map4d.CameraPosition([10, 108], tilt, bearing, zoom)
 map.setSwitchMode(map4d.SwitchMode.Auto)
 ```

## 11. Cài đặt hiệu ứng thời tiết
Map4D SDK cho phép người dùng cài đặt các hiệu ứng theo thời gian thực hoặc thủ công
```javascript
map.setWeather(Weather.Rain)
getWeather(): Weather
```
- **setWeather**: Cài đặt hiệu ứng thời tiết
  - *Weather.Rain*: Hiệu ứng thời tiết trời đang mưa
  - *Weather.Snow*: Hiệu ứng thời tiết tuyết đang rơi
  - *Weather.Sun*: Hiệu ứng thời tiết trời nắng
  
- **getWeather**: Trả về thông tin thời tiết hiện tại

License
-------

Copyright (C) 2016 IOT Link Ltd. All Rights Reserved.
