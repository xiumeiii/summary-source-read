```html
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" 
"http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <circle cx="100" cy="50" r="40" stroke="black"
  stroke-width="2" fill="red" />
</svg>
```
- standalone : SVG 文件是否是"独立的",standalone="no" 意味着 SVG 文档会引用一个外部文件，是 DTD 文件。该 DTD 位于 "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd"。
- SVG 代码以 <svg> 元素开始，包括开启标签 <svg> 和关闭标签 </svg> 。
- 注释：所有的开启标签必须有关闭标签！

SVG 在 HTML 页面：
1. <embed>
2. <object>
3. <iframe>
4. 直接在HTML嵌入SVG代码
5. 链接到SVG文件  
  ```html <a href="circle1.svg">View SVG file</a> ```

SVG有一些预定义的形状元素即shape形状类元素:
- 矩形 <rect>
- 圆形 <circle>
- 椭圆 <ellipse>
- 线 <line>
- 折线 <polyline>
- 多边形 <polygon>
- 路径 <path>
