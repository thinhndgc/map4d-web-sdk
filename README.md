# Map4D SDK

Map4D SDK for Web, written in Javascript.

[![CocoaPods](https://raw.githubusercontent.com/iotlinkadmin/map4d-web-sdk/master/sdk/map4dweb.png)](https://map4d.vn) 


## Installation

Use [NPM](https://www.npmjs.com).

```npm
npm install map4dsdk --save
```
## Using

1. Initialize map with access key

```html
<body>
<div id="map"></div>

<script src="[PATH]/map4dsdk@[version].js"></script>

<script>
  let map = new map4d.Map(document.getElementById("map"),
      {
        zoom: 13,
        center: [10.793113, 106.720739],
        geolocate: true,
        accessKey: "98fd21346d83bee24dc734231f7609c9"
      }
    )
</script>

</body>
```

2. Style your view

```css
  <style>
	body {
	  margin: 0px;
	  height: 100%;
	}

	#map {
	  position: absolute;
	  width: 100%;
	  height: 100%;
	  background-color: #f2efe9;
	}
  </style>
  ```

License
-------

Copyright (C) 2016 IOT Link Ltd. All Rights Reserved.
