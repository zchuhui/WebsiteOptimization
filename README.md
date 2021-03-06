## 网站站点优化demo
尽可能优化这个在线项目的速度与动画效果。

## 优化手段
使用 `Chrome devs`工具的`Performance`查看并分析站点的性能问题，辅助使用Chrome插件 `PageSpeed`查看 `PageSpeed` 分数与优化建议。


## 检查站点问题及解决方案
#### 1.网站index页面卡顿严重
解决方案：   
1.卡顿问题有`html`标签使用不规范以及图片太大导致，所以优化图片大小，压缩图片，并规范化`html`标签。   
2.由于css文件不多，所用用内嵌的方式取代外联。而print.css,设置 `media='print'`，让其在打印的时候调用。

#### 2.网站pizza页面背景动画有卡顿现象，10帧的平均帧率偏高，滚动帧速会跟随环境变化，
解决方案：   
缓存DOM操作并把循环中可以提起的操作提出来，而不是每次循环都提取一遍。   
用requestAnimationFrame来实现动画，并设置 `60fps` 的帧速

```
// 先缓存dom
var items = document.querySelectorAll('.mover');
var scroll_top = document.body.scrollTop / 1250;


// 操作帧的参数，保持 60fps 的帧速
var fps = 60;
var now;
var then = Date.now();
var interval = 1000/fps;
var delta;
var frameId;

function moveLeft(){
// 使用requestAnimationFrame绘制
frameId = requestAnimationFrame(moveLeft);

now = Date.now();
delta = now - then;

if (delta > interval) {

  then = now - (delta % interval);

  for (var i = 0,len = items.length; i < len; i++) {
    var phase = Math.sin(scroll_top + (i % 5));
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';

    // 子元素都绘制好后，停止requestAnimationFrame
    if (i == len-1) {
      cancelAnimationFrame(frameId);
    }
    
  }
}
}

moveLeft();

```

#### 3.pizza尺寸缩放渲染时间太长的问题
解决方案：   
循环比较大的问题,能一遍提取或计算的，不要放在循环里重复操作。

```
// 遍历披萨的元素并改变它们的宽度
function changePizzaSizes(size) {

var _pizza = document.querySelectorAll(".randomPizzaContainer");

// 这些值只要取一遍就行了
var dx = determineDx(_pizza[0], size);
var newwidth = (_pizza[0].offsetWidth + dx) + 'px';  

for (var i = 0; i < _pizza.length; i++) {
  _pizza[i].style.width = newwidth;       
}

}
```



   
## 启动服务

1.安装`Python`环境

2.启动服务
```
python -m http.server 8000

```
3.打开浏览器：localhost:8000


## 关于Page Speed 评分
### index.html页面   
PC端：
![image](https://picabstract-preview-ftn.weiyun.com:8443/ftn_pic_abs_v2/5d7743c99775085bdf391a64651be4a2a23fe9db48faf959b2a037378e36164e0ba593bbeafda0fd26b6023cbbbcc128?pictype=scale&from=30113&version=2.0.0.2&uin=943339564&fname=20170520002002.png&size=1024)

移动端：  
![image](https://picabstract-preview-ftn.weiyun.com:8443/ftn_pic_abs_v2/0e0003a38c42f6d422a2aa18a0ddb8deb107cc317c59611f164706ca014242c28db07619063b5c9d55abaca49605d7eb?pictype=scale&from=30113&version=2.0.0.2&uin=943339564&fname=20170520002244.png&size=1024)

 

