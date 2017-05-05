###面向对象的方法实现万年历

实现思路：

   1.创建构造函数constructor

   ```
   function Calender(main){
		this.currentDate = new Date()
		this.main = main;
	}
   ```
   2.创建日历对象的prototype方法
   ```
   Calender.prototype = {
		constructor: Calender,
		//创建主体内容区域
		showMain:function(){},

		//创建标题头区域
		showHeader: function(){},	

		//创建week标题头
		showWeek: function(){},	

		//创建容纳日期的容器
		showDate:function(){},

		//封装一个方法，待点击切换月份时，初始化页面
		showInit: function(){}
	};//对象结束
   ```

   3.实现思路：
   
     ==> 创建日历对象的主体内容   
``` 
       showMain:function(){
			var $t = this;//将this对象缓存到$t中,this指向Calender对象
			
			$t.main = document.createElement("main")
			document.body.appendChild($t.main);
			$t.main.style.width = "600px";
			$t.main.style.height = "600px";
			$t.main.style.backgroundColor = "gray";
			$t.main.style.margin = "50px auto";
			$t.main.style.borderRadius = '15px'
		}
```

	 ==> 创建标题头区域（包含切换按钮/年月）
``` 
       showHeader: function(){
			var $t = this;
			//左控制键
			var prevSpan = document.createElement('span')
			$t.main.appendChild(prevSpan)
			prevSpan.innerHTML = '&lt;'
			
			//存放年月信息
			var span = document.createElement('span')
			$t.main.appendChild(span)
			
			//右控制键
			var nextSpan = document.createElement('span')
			$t.main.appendChild(nextSpan)
			nextSpan.innerHTML = '&gt;'
			
			var allSpan = document.querySelectorAll('span')
			var month = $t.currentDate.getMonth() + 1
			var year = $t.currentDate.getFullYear()
			
			//设置左边年月信息
			span.innerHTML = year + '年' + month + '月'

			for(var i = 0; i < allSpan.length; i++) {
				var span = allSpan[i]
				//设置span的css样式
				span.style.display = 'inline-block'
				span.style.width = '33.3%';
				span.style.height = '100px'
				span.style.color = 'white'
				span.style.fontSize = '30px'
				span.style.textAlign = 'center'
				span.style.lineHeight = '100px'
				span.id = 'index' + i
				
				//为左右按键添加事件
				if(i==0 || i==2) {
					span.onclick = function() {
						month = $t.currentDate.getMonth()
						year = $t.currentDate.getFullYear()
						if(this.id == 'index0') {
							month--
						} else if(this.id == "index2") {
							month++
						}
					//点击完重新设置当前月
					$t.currentDate.setMonth(month)

					//清空页面
					document.body.innerHTML = ''
					//重新布局
					$t.showInit()
					}
				}
			};		
		}
```

     ==> 创建week标题头
``` 
      showWeek: function(){
			var $t = this;
			//创建table标签
			var table = document.createElement('table')
			$t.main.appendChild(table)
			table.style.width = '100%'
			//往表格里边添加一行
			var tr = table.insertRow()
			var weeks = ['日','一', '二', '三', '四', '五', '六' ]

			for(var i = 0; i < weeks.length; i++) {
				//往tr插入列
				var td = tr.insertCell()
				td.style.height = '50px'
				td.style.backgroundColor = 'goldenrod'
				td.style.textAlign = 'center'
				td.innerHTML = weeks[i]
			}
		},
```

      ==> 创建容纳日期的容器 
```
      showDate:function(){
			var $t = this;
			var table = document.createElement('table')
			$t.main.appendChild(table)
			table.style.width = '100%'
			
			//创建一个名言标签
			var  p = document.createElement("p")
			$t.main.appendChild(p)
			p.innerHTML = "每一天都是新的自己，怎么活，自己决定！"
			p.style.fontSize = "24px";
			p.style.width = "100%";
			p.style.height = "60px";
			p.style.lineHeight = "30px"
			p.style.textAlign = "center"
			p.style.color = "white"
			
			//创建日期容器
			for(var i = 0; i < 6; i++) {
				//创建6行
				var tr = table.insertRow()
				for(var k = 0; k < 7; k++) {
					//每一行7格
					var td = tr.insertCell()
					td.style.height = '60px'
					td.style.backgroundColor = 'orange'
					td.style.textAlign = 'center'
					td.style.fontSize = "20px"
					var dayIndex = i * 7 + k
					
					//计算每一个格所对应的日期对象
					var eachDate = getEveryDate(dayIndex)
					//得到每一个日期的号数
					td.innerHTML = eachDate.getDate()
					
					//获取当天日期的值,突出显示当天日期
					var now = new Date()
					var currentDay = now.getDate()
                    //console.log(currentDay)
					if(eachDate.getMonth() == now.getMonth() && eachDate.getDate() ===currentDay)
						td.style.backgroundColor = "aquamarine"
					
					//判断当前月和推算出来的日期的月份是否一样，不一样则代表不是当前月的日期，则把日期灰色显示
					if(eachDate.getMonth() != $t.currentDate.getMonth()) {
						td.style.backgroundColor = 'lightgray'
						td.style.color = 'gray'
					}
				}
			}
		},
```

	==> 封装一个方法，待点击切换月份时，初始化页面 
``` 
      //封装一个方法，待点击切换月份时，初始化页面
		showInit: function(){
			var $t = this;
			$t.showMain()
			$t.showHeader()
			$t.showWeek()
			$t.showDate()
		}
```

```  ==> 对象实例化和初始化页面
        //实例化一个Calender对象
	    var calender = new Calender() 
	
	    calender.showInit()//初始化页面
```
     
     