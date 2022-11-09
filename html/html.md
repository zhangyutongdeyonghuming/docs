
## 事件声明

> input onchange事件

```html
<!DOCTYPE html>

<html lang="en">

  

<head>

    <meta charset="UTF-8">

    <meta http-equiv="X-UA-Compatible" content="IE=edge">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Document</title>

</head>

  

<body>

    <input type="text" onchange="f(event)">

</body>
  
<script>
    function f(event) {

        console.log(event.target.value)

    }
</script>

  

</html>
```

![](https://raw.githubusercontent.com/zhangyutongdeyonghuming/images/main/202210270959332.png?token=AMWBGOTLRTQQKTH4NJM3D73DLHTFC)

## 文件下载

> js版本
```javascript
/**  
 * 下载文件  
 * @param fileUrl 文件url  
 * @param fileName 文件name  
 */
 const download = (fileUrl, fileName) => {  
    const x = new window.XMLHttpRequest();  
    x.open('GET', fileUrl, true);  
    x.responseType = 'blob';  
    x.onload = () => {  
        const url = window.URL.createObjectURL(x.response);  
        const a = document.createElement('a');  
        a.href = url;  
        a.download = fileName;  
        a.click();  
    };  
    x.send();  
}
```

> a标签版本
```html
<a href="${url}?response-content-type=application/octet-stream" download target="_target">点击下载</a>
```

## 冒泡事件

> 内层dom事件触发时同时触发了外层dom, 如果不需要的话, 阻止冒泡即可

```html
<!DOCTYPE html>

<html lang="en">

  

<head>

    <meta charset="UTF-8">

    <meta http-equiv="X-UA-Compatible" content="IE=edge">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Document</title>

</head>
<body>
    <!-- 外层 -->

    <div onclick="f1(event, 2)">

        <!-- 内层 -->

        <button onclick="f2(event,2 )">111</button>

    </div>

</body>

<script>

    const f1 = (e) => {

        alert(1)

    }

    const f2 = (e) => {

        alert(2)
		// 阻止
        e.stopPropagation()

    }

</script>

  

</html>
```
![](https://raw.githubusercontent.com/zhangyutongdeyonghuming/images/main/202211042234012.png?token=AMWBGOXLPBMS4NH26MSAJO3DMURUA)
![](https://raw.githubusercontent.com/zhangyutongdeyonghuming/images/main/202211042234012.png?token=AMWBGOXLPBMS4NH26MSAJO3DMURUA)
![](https://raw.githubusercontent.com/zhangyutongdeyonghuming/images/main/202211042234012.png?token=AMWBGOXLPBMS4NH26MSAJO3DMURUA)
![](https://raw.githubusercontent.com/zhangyutongdeyonghuming/images/main/202211042235553.png?token=AMWBGOUHR5EYEUVLPICMX33DMURVY)
![](https://raw.githubusercontent.com/zhangyutongdeyonghuming/images/main/202211042235553.png?token=AMWBGOUHR5EYEUVLPICMX33DMURVY)
