# 替换型元素

将文件的内容引入，替换掉自身位置的一类标签。

- `script`
- `img`
- `pickture`
- `audio`
- `video`
- `iframe`

## 替换型元素和链接型元素的区别

凡是替换型元素，都是使用`src`来引用文件，链接型元素都使用`href`标签。

## script

既可以作为替换型标签，又可以不作为替换型标签的元素。
**用法**：两种

- 直接把脚本代码写在 script 标签之间，
- 是把代码放到独立的 js 文件中，用 src 属性引入。

## img

### **引入图片**：两种

- src 属性
- data uri

```
<img src='data:image/svg+xml;charset=utf8,<svg version="1.1" xmlns="http://www.w3.org/2000/svg"><rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:1;stroke:rgb(0,0,0)"/></svg>'/>
```

### 属性

- `src` 图片链接
- `alt` 图片加载未完成时显示的替换文字，
- `width`/`height` 宽、高，只设置一个等比例缩放
- `srcset`和`sizes` 在不同屏幕大小和特性下，使用不同图片源

```htmlbars
<img srcset="elva-fairy-320w.jpg 320w,
             elva-fairy-480w.jpg 480w,
             elva-fairy-800w.jpg 800w"
     sizes="(max-width: 320px) 280px,
            (max-width: 480px) 440px,
            800px"
     src="elva-fairy-800w.jpg" alt="Elva dressed as a fairy">

```

## picture

picture 元素可以根据屏幕的条件为其中的 img 元素提供不同的源，使用`source`元素来指定图片源，它的基本用法如下：

```
<picture>
  <source srcset="image-wide.png" media="(min-width: 600px)">
  <img src="image-narrow.png">
</picture>
```

## video

视频元素。使用`source`指定不同播放源

```htmlbars
<video controls="controls" >
  <source src="movie.webm" type="video/webm" >
  <source src="movie.ogg" type="video/ogg" >
  <source src="movie.mp4" type="video/mp4">
  You browser does not support video.
</video>

```

### track 播放时序相关的标签

**用途**：字幕，使用`srclang`来指定语言

#### track 元素`kind`属性

- subtitles：就是字幕了，不一定是翻译，也可能是补充性说明
- captions：报幕内容，可能包含演职员表等元信息，适合听障人士或者没有打开声音的人了解音频内容
- descriptions：视频描述信息，适合视障人士或者没 有视频播放功能的终端打开视频时了解视频内尔用
- chapters：用于浏览器视频内容。
- metadata：给代码提供的元信息，对普通用户不可见。

## audio

可以使用`<source>`标签或者`src`属性

## iframe

嵌入一个完整的网页

```
<iframe sandbox srcdoc="<p>Yeah, you can see it <a href="/gallery?mode=cover&amp;amp;page=1">in my gallery</a>."></iframe>
```

### 解决跨域

在新标准中，为 iframe 加入了 sandbox 模式和 srcdoc 属性.这个例子中，使用 srcdoc 属性创建了一个新的文档，嵌入在 iframe 中展示，并且使用了 sandbox 来隔离。

```
<iframe sandbox srcdoc="<p>Yeah, you can see it <a href="/gallery?mode=cover&amp;amp;page=1">in my gallery</a>."></iframe>
```
