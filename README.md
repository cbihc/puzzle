# puzzle
<html>
	<head>
    	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        
    	<title></title>
    </head>
    <style>
		#table{
			width:240px;
			height:240px;
			position:absolute;
			top:100px;
			left:100px;	
			border:1px solid #999;
		}
		
		tr{
			height:60px;
			border:1px ssolid red;
		}
		td{
			width:60px;
			border:1px ssolid red;
		}
		
		div{
			width:60px;
			height:60px;
			position:absolute;	
		}
		#img1{
			position:absolute;
			left:100px;
			top:400px;
			border:1px red solid;
			width:240px;
			height:240px;	
		}
    </style>
    <script src="run.js"></script>
    <script>
    	window.onload = function(){
			var aDiv=document.getElementsByTagName("div");
			
			
			for(var i=0; i<aDiv.length; i++){
				aDiv[i].style.left =parseInt(400+(Math.random()*300));	
				aDiv[i].style.top =parseInt(100+(Math.random()*200));	
			}
			function starDrag(obj,oEvent){
				obj.dx=oEvent.clientX-css(obj,'left');
				obj.dy=oEvent.clientY-css(obj,'top');
				
				document.onmousemove=function(ev){
					var oEvent =ev ||event;
					obj.style.left=oEvent.clientX-obj.dx;
					obj.style.top =oEvent.clientY-obj.dy;
					document.title = css(obj,'top');
	
				}	
			}	
			
			function stopDrag(obj){
				document.onmousemove = null;
				
			}
			
			for(var i=0; i<aDiv.length; i++){
				aDiv[i].index=i;
				aDiv[i].onmousedown = function(ev){
				var oEvent = ev || event;
				starDrag(this,oEvent);
				}
			
			
			
			aDiv[i].onmouseup = function(){
				stopDrag(this);
				var le=css(this, 'left');
				var to=css(this, 'top');
				if(le>=100 && le<=340 &&to>=100 &&to<=340){
					this.style.left =parseInt((le-100)/60)*60+100;	
					this.style.top =parseInt((to-100)/60)*60+100;	
				}	
			}
			}

		}
	</script>
	<body>
        	<table border="1" cellspacing="0" id="table">
            	<tr>
                	<td></td>
                    <td></td>
                    <td></td>
                    <td></td>
                </tr>
                <tr>
                	<td></td>
                    <td></td>
                    <td></td>
                    <td></td> 
                </tr>
                <tr>
                	<td></td>
                    <td></td>
                    <td></td>
                    <td></td>
                </tr>
                <tr>
                	<td></td>
                    <td></td>
                    <td></td>
                    <td></td>
                </tr>
                
            </table>
      
       
        	<div style="background:url(images/2.jpg) no-repeat;"></div>
       		<div style="background:url(images/3.jpg) no-repeat;"></div>
            <div style="background:url(images/4.jpg) no-repeat;"></div>
            <div style="background:url(images/5.jpg) no-repeat;"></div>
            <div style="background:url(images/6.jpg) no-repeat;"></div>
            <div style="background:url(images/7.jpg) no-repeat;"></div>
            <div style="background:url(images/8.jpg) no-repeat;"></div>
            <div style="background:url(images/9.jpg) no-repeat;"></div>
            <div style="background:url(images/10.jpg) no-repeat;"></div>
            <div style="background:url(images/11.jpg) no-repeat;"></div>
            <div style="background:url(images/12.jpg) no-repeat;"></div>
            <div style="background:url(images/13.jpg) no-repeat;"></div>
            <div style="background:url(images/14.jpg) no-repeat;"></div>
            <div style="background:url(images/15.jpg) no-repeat;"></div>
            <div style="background:url(images/16.jpg) no-repeat;"></div>
            <div style="background:url(images/17.jpg) no-repeat;"></div>
      
        <p id="img1"><img src="images/1.jpg"></p>

    </body>
</html>


 //下面是调用js代码
 // JavaScript Document
function css(obj,attr,value){
	if(arguments.length == 2){
		if(attr == "opacity"){
			if(obj.currentStyle){
				return parseInt(obj.currentStyle[attr]*100);
			} else {
				return parseInt(getComputedStyle(obj,false)[attr]*100);
			}
		} else {
			if(obj.currentStyle){
				return parseInt(obj.currentStyle[attr]);
			} else {
				return parseInt(getComputedStyle(obj,false)[attr]);
			}
		}
	} else {
		if(attr == "opacity"){
			obj.style.opacity = value/100;
			obj.style.filter = "alpha(opacity:"+value+")";
		} else {
			obj.style[attr] = value;
		}
	}
}

function run(obj,json,model,endFunction){               //运动框架
	
		//attr:属性的名称,一组属性
		//json[attr]:相当于原来的iTarget,一组目标值
	
	var speed = 5;    //确定方向，即确定speed的值
	
	
		function runOne(){   //单次运动的函数
			for(var attr in json){
			if(model == 0){ //匀速运动
				if(json[attr] > css(obj,attr)){
					speed = 5;
				} else if(json[attr] <css(obj,attr)){
					speed = -5;
				} else {
					speed = 0;
				}
			} else {  //缓冲运动
				speed = (json[attr] - css(obj,attr))/10;
				if(speed >0){
					speed = Math.ceil(speed);  //向上取整
				} else if(speed<0){
					speed = Math.floor(speed);  //向下取整
				}
			}
				
				//obj.style[attr] = css(obj,attr) + speed;  //走
			//alert(css(obj,attr) + ":" + speed);
			css(obj,attr,css(obj,attr)+speed);
			
			if(css(obj,attr) ==json[attr] ||  Math.abs(json[attr] - css(obj,attr))<Math.abs(speed) ){				//到目标点后，停
					//obj.style[attr] = iTarget;
				css(obj,attr,json[attr]);
				//clearInterval(obj.timer);
				if(endFunction){
					endFunction();
				}
			}
		}
	}
	
	var flag = true; //假设所有要改变的属性，都达到目标值了
	
	for(var attr in json){
		if(css(obj,attr) != json[attr]){
			flag = false;
		}
	}
	
	if(flag){
		clearInterval(obj.timer);
	}
	
	
	clearInterval(obj.timer);       //开启之前先关闭，避免重复开启
	obj.timer = setInterval(runOne,30);  //重复执行单次运动，以达到运动效果
	
}

function scrollInfo(){
		var scrollTop=document.documentElement.scrollTop || document.body.scrollTop;
		var scrollLeft=document.documentElement.scrollLeft || document.body.scrollLeft;
		
		var pageWidth = document.body.scrollWidth;//页面的范围的宽度
		var pageHeight = document.body.scrollHeight;//页面的范围的高度
		
		
		var scrollWidth=window.innerWidth|| document.documentElement.scrollWidth;
		var scrollHeight=window.innerHeight|| document.documentElement.scrollHeight;
		
		return{t:scrollTop,l:scrollLeft,w:scrollWidth,h:scrollHeight,pw:pageWidth, ph:pageHeight};
	}
	
	
	function Drag(id){//对像的应用js代码
					this.id=document.getElementById(id);
					this.dx=0;
					this.dy=0;
					var _t=this;
					
				 	this.id.onmousedown=function(ev){
						
						_t.fnDown(ev);	
						document.onmousemove=function(ev){
							
							_t.fnMove(ev);
						}
						document.onmouseup = function(){
							_t.fnUp();
						}
					}
									
				}
				
				Drag.prototype.fnDown=function(ev){
					
					var oEvent = ev || event;
					this.dx=oEvent.clientX-this.id.offsetLeft;
					
					this.dy=oEvent.clientY-this.id.offsetTop;
					var _t=this;
					
					
				}
				
				Drag.prototype.fnMove=function(ev){
					
					var oEvent = ev || event;
					this.id.style.left=oEvent.clientX-this.dx;
					this.id.style.top= oEvent.clientY-this.dy;	
				}
				Drag.prototype.fnUp = function(){
					
					document.onmousemove=null;	
					document.onmouseup=null;	
				}
				
				
				function NewDrag(id) {
					Drag.call(this,id);
					
				}
				
				for(var i in Drag.prototype) {
					NewDrag.prototype[i] = Drag.prototype[i];	
				}
				
				NewDrag.prototype.fnMove = function(ev) {
					
					var oEvent = ev|| event;
					var sc = scrollInfo();
					this.id.style.left=oEvent.clientX-this.dx;
					this.id.style.top= oEvent.clientY-this.dy;	
					
					
					if(css(this.id,'left') < 0){ 
						this.id.style.left = 0;	
					}
					
					if(css(this.id,'top') < 0){ 
						this.id.style.top = 0;	
					}
					
					if(css(this.id, 'left') > sc.w - css(this.id, 'width')){
						this.id.style.left =sc.w - css(this.id, 'width');	
					}
					
					if(css(this.id,'top')>sc.h-css(this.id, 'height')){
						this.id.style.top =sc.h-css(this.id, 'height');	
					}
					
				}
				
				
		function setCookie(attr,value,date){
			var oDate =new Date();
			oDate.setDate(oDate.getDate()+date);	
			document.cookie=attr+"="+value+";expires="+oDate;
				
		};
			
		function removerCookie(attr){
			setCookie(attr,1,-1);
		}
		
		
		function getCookie(attr){
				var str=document.cookie;
				var strArr=str.split('; ');
				for(var i=0; i<strArr.length; i++){
					var tmpArr=strArr[i].split('=');
					if(tmpArr[0]==attr){
						return tmpArr[1];
					}	
				}
				return '';		
			}
