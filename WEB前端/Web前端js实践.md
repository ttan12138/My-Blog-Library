#### 简单JS实践

##### 计时器

目标：用简单的html+css+js实现一个支持网页计时的小工具。

所需知识：js&html&css基础以及DOM中的setInterval() 方法

注：setInterval()方法可按照指定的周期（以毫秒计）来调用函数或计算表达式。

​        setInterval() 方法会不停地调用函数，直到 clearInterval() 被调用或窗口被关闭。

> index.html

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>网页计时器</title>
        <link rel="stylesheet" type="text/css" href="index.css"/>
	</head>
	<body>
		<div id="timer">
			<p id="name">计时器  &nbsp;   <span id="timeDate"></span>  </p>
			<br>
			<div id="timeView">00:00:00</div>
			<button class="box" type="button" id="start">RESTART</button>
			<br>
      <button class="box" type="button" id="continue">CONTINUE</button>
			<br>
      <button class="box" type="button" id="stop">STOP</button>
			<br>
			<button class="box" type="button" id="reset">RESET</button>
			<br>
		</div>
        <script type="text/javascript" src="index.js"></script>
	</body>
</html>

```

> index.css

```css
div .box {
  font-weight: bold;
  background-color: aquamarine;
  margin-left: 150px;
  margin-bottom: 30px;
  text-align: center;
  line-height:65px ;
  height: 65px ;
  width: 180px;
}
#timer{
  margin: auto;
  height: 750px;
  width: 500px;
  background-color: yellowgreen;
}
#timeView{
  margin-bottom: 50px;
  margin-left: 20px;
  margin-right: 20px;
  font-weight:bold;
  text-align: center;
  line-height:250px;
  height: 250px;
  width: 400x;
  background-color: #000000;
  font-size: 50px;
  color: white;
}
#name{
  margin: 0;
  font-weight:bold;
  text-align: left;
  font-size: 18px;
  color: greenyellow;
}
#timeDate{
  margin: 3px;
  font-size: 18px;
  line-height:18px;
  float: right;
}

```

> index.js

```js
window.onload = function (){
    var oDiv = document.getElementById("timeView");
    var oStart = document.getElementById("start");
    var oStop = document.getElementById("stop");
    var oReset = document.getElementById("reset");
    var oSpan = document.getElementById("timeDate");
    var oContinue = document.getElementById("continue");
    
    var oTime = null;  //设置计时器为oTime
    var start = false;  //设置计时器开始标识
    var con = false;  //设置计时器继续标识
    var count = 0;  //设置单个计时器总秒数
    
    window.setInterval(function(){
      var d = new Date();
      oSpan.innerHTML = oRight(d.getUTCFullYear())+"年"+oRight(d.getMonth()+1)+"月"+oRight(d.getDate())+"日"+"  "+oRight(d.getHours())+":"+oRight(d.getMinutes())+":"+oRight(d.getSeconds());
      oStart.onclick = function(){
        if(!start){
          start = true;
          oTime = setInterval(function(){
            count++;
            oDiv.innerHTML = oRight(parseInt(count / 3600))+":"+oRight(parseInt((count / 60) % 60))+":"+oRight(count % 60);
          }, 1000);
        }else{
          count = 0;
        }
      };
      
      oStop.onclick = function(){
        start = false; //设置为已经开始
        con = true;	//设置为可以暂停
        clearInterval(oTime);//清楚计时器，即停止
      };
            
      oReset.onclick = function(){
        start = false;
        clearInterval(oTime);
        count = 0 ;
        oDiv.innerHTML = "00:00:00";
      };
      
      oContinue.onclick = function() {
        if(!start && con){
          oTime = setInterval(function(){
            count++;
            oDiv.innerHTML = oRight(parseInt(count / 3600))+":"+oRight(parseInt((count / 60) % 60))+":"+oRight(count % 60);//将总秒数转换为对应显示
            con=false;
          }, 1000);
        }else if(!count){
          alert("当前没有计时");
        }else if(start){
          alert("计时器已经在计时了");
        }else if(!con){
          alert("已经在继续了");
        }
        
      }
      //一位数用 0 占十位
      function oRight(num){
        if(num < 10){
          return "0"+num;
        }else{
          return num;
        }
      }
    },1000);
    
}
```

可拓展：设置记录按钮，多次点击“记录”按钮可以不停止计时，记下每次点击时的成绩并显示在界面对应区域。点击“RESTART”时可以清除记录重新开始。





