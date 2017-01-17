date: 2017-01-17 18:40:02
---

---
图片占位符，图片由于诸如过大导致浏览器渲染慢、服务器端响应时间过长、页面需要加载的图片过多等原因导致图片未加载。
某些图片未成功渲染场景下，希望页面结构能大致保持不变。

## 一、简介

Holder renders image placeholders entirely in browser.

Placeholders can have custom colors, fonts, resizing behavior, and rendering engine (Canvas/SVG).

## 二、使用步骤

### 1. 引用类库

```javascript
<script src="js/holder.js"></script>
```

### 2. 使用方法

#### Programmatic use API to process image: placeholder.getData(opts)

通过js程序设置图片src内容。

```javascript
document.getElementById('img_holder').setAttribute('src', window.placeholder.getData(opts));
```

#### Image onerror event

在img标签上添加onerror监听事件。

```html
onerror="this.src=placeholder.getData({size: '256x128', text: 'Image 404'})"
```

#### Use image config attribute.

```html
<img class="placeholder">
<img options="size=256x128&text=Hello!" class="placeholder">
<img options="size=256x128&text=Hello%2525%26%3DWorld" class="placeholder">
```

这种方式，有几点使用注意事项：

- 需要在所有img标签后添加渲染语句

```javascript
<script type="text/javascript">
// 使用第3类方式需要，执行此方法
placeholder.render()
</script>
```

- options内text值需要做encodeURIComponent
- img标签必须包含名为placeholder的class属性值

## 三、实例

```html
<!DOCTYPE html>
<html>

<head>
  <title>placeholder实例</title>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <script src="http://cdn.bootcss.com/placeholder.js/3.1.0/placeholder.js"></script>
</head>

<body>
  <p>1. Programmatic use API to process image: placeholder.getData(opts)</p>
  <pre>document.getElementById('img_holder').setAttribute('src', window.placeholder.getData(opts));</pre>
  <img id="img_holder" src="">
  <script type="text/javascript">
  var opts = {
    size: '256x128',
    font: {
      style: 'oblique',
    },
    text: 'Hello World'
  }
  document.getElementById('img_holder').setAttribute('src', window.placeholder.getData(opts));
  </script>
  <p>2. Image onerror event</p>
  <pre>onerror="this.src=placeholder.getData({size: '256x128', text: 'Image 404'})"</pre>
  <img id="img_holder2" src="404.png" onerror="this.src=placeholder.getData({size: '256x128', text: 'Image 404'})">

  <p>3. Use image config attribute.</p>
  <pre>&lt;img class="placeholder"&gt;</pre>

  <img class="placeholder">
  <pre>&lt;img options="size=256x128&text=Hello!" class="placeholder"&gt;</pre>

  <img options="size=256x128&text=Hello!" class="placeholder">
  <pre>&lt;img options="size=256x128&text=Hello!&bgcolor=#ccc&color=#969696&fstyle=oblique&fweight=bold&fsize=40&ffamily=consolas" class="placeholder"&gt;</pre>
  <img options="size=256x128&text=Hello!&bgcolor=#ccc&color=#969696&fstyle=oblique&fweight=bold&fsize=40&ffamily=consolas" class="placeholder">

  <script type="text/javascript">
  // 使用第3类方式需要，执行此方法
  placeholder.render()
  </script>

</body>

</html>
```

## 四、参考文档

官方地址：http://placeholder.cn/
使用实例：http://placeholder.cn/doc/demo.html

