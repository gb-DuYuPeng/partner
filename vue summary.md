

## vue 知识点总结

[TOC]



#### 一，实现第一个Vue页面

​	01.下载文件并引入 引入之后，在我们浏览器的内存中，多了一个Vue构造函数。

```
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.5.17-beta.0/vue.min.js"></script>
```

​	02.实例化vue应用程序 new Vue({参数配置})

​	03.通过配置el去关联视图层 ---即用户能看到的页面

​	04.通过配置data去设置相关的参数属性，即储存数据

​	05.通过{{}} 表达式在视图层里渲染数据。

```
<!-- 这是 MVVM中的 V view 视图 -->
<div class="app">
		<h1>{{ msg }}</h1>
	</div>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.5.17-beta.0/vue.min.js"></script>
<script type="text/javascript">
 	//这是 MVVM中的 VM viewmodel 视图模型
 	let vm = new Vue({
 		//el 就是element 元素的意思 ，其作用：与页面关联的
 		el:'.app',
 		//这里data就是MVVM中的 M model 数据模型，专门储存数据的
 		data:{
 			msg:'hello world'
 		}
 	})
</script>
```



#### 二，通过{{}} 表达式在视图层里渲染数据，在刷新页面时，会出现闪烁现象。

​	解决： 	① 使用 v-cloak ，设置样式。

```
[v-cloak]{display: none;}
<h1 v-cloak>{{msg}}</h1>		
```

​			②  使用 v-text或者v-html

```
<h2 v-text = 'msg'>=====</h2>
<h2 v-html = 'msg'>=====</h2>
```

​	区别：	后者会覆盖元素里原本的内容

#### 三，v-if与v-show的异同和使用场景。

​	v-if =‘ true或者false ’

​	v-show=‘ true或者false ’

​	相同点：都可以动态控制元素显示和隐藏
	区别：     	 v-if 将整个dom元素添加或者删除  	 v-show 显示和隐藏
	性能消耗：	 v-if 有更高的切换的消耗          		 v-show 有更高的渲染消耗
	使用场景： 	 v-if 适合运营条件改变不大的时候 	 v-show 适合频繁切换

#### 四，v-model与v-bind。

​	v-model 用于 input 标签，在input元素上进行双向绑定数据

​	v-bind用于单向绑定属性和数据 ，其缩写为“ : ” 也就是v-bind:id  === :id  

#### 五，v-on与methods

```
<div id="App">
    	// mygame 是属性值
    <p>{{mygame}}</p>
 		// v-on:click="clickme('do you like game')" v-on 监听事件,click是点击事件类型
    <button v-on:click="clickme('do you like game ?')" >no</button>
		// @click事件监听简写@click="clickme('yes iam like game')"
    <button @click="clickme('yes i am like game')" > yes</button>
</div>
	var App = new Vue({
        el: "#App",
        data: { //数据区data
            mygame: "超级马里奥",
        },
        methods: { //函数区 methods
            clickme: function (value) {
                this.mygame = value;
            },
        },
    });
```

#### 六，v-for以及v-for中的key值

​	v-for 根据一组数组的选项列表进行渲染
	语法： v-for = "item in arr"
	item: 数组里的每一个元素
	arr： 数组 

```
<p v-for='item in list' :key='item.id'>
	<input type="checkbox">
	{{item.id}}==={{item.name}}
</p>
let vm = new Vue({
	el:'#app',
	data:{
		list:[
			{id:1,name:'小明'},
			{id:2,name:'小红'},
			{id:3,name:'小黑'},
			{id:4,name:'小白'},
		]
	},
})
```

**:key='item.id'**  ==>	便于能跟踪每个节点的身份，从而重用和重新排序现有元素，需要为每项提供一个唯一 

​				key 属性。理想的 key 值是每项都有的且唯一的 id。 但它的工作方式类似于一个属性，

​				所以你需要用 v-bind 来绑定动态值 

#### 七，事件修饰符

- .stop       阻止冒泡
- .prevent    阻止默认事件
- .capture    添加事件侦听器时使用事件捕获模式
- .self       只当事件在该元素本身（比如不是子元素）触发时触发回调
- .once       事件只触发一次

```
	<div id="app">
			<!-- .stop阻止别人的冒泡 点击btn时，只会触发btn（）事件，
					而不会因为冒泡而触发div（）事件-->
		<div class="myDiv" @click = 'div（）'>
			<button @click.stop = 'btn'>点击</button>
		</div>
			<!-- .prevent  阻止默认事件  点击a标签时，页面不会跳转到http://www.baidu.com-->
		<a href="http://www.baidu.com" @click.prevent = 'aClick'></a>
			<!-- .capture    添加事件侦听器时使用事件捕获模式  触发顺序 
							点击btn时，div（）方法先触发，btn（）方法后触发-->
		<div class="myDiv" @click.capture = 'div（）'>
			<button @click = 'btn'>点击</button>
		</div>
			<!-- .self  只会阻止自身的冒泡行为
            		点击btn时，会触发btn（）和 outDiv（）事件，而不会触发 Div（）事件-->
		<div class="outDiv" @click = 'outDiv（）'>
			<div class="myDiv" @click.self = 'div（）'>
				<button @click = 'btn（）'>点击</button>
			</div>
		</div>
			<!-- .once  事件只触发一次  阻止一次页面跳转事件-->
		<a href="http://www.baidu.com" @click.prevent.once = 'aClick'></a>
	</div>
```

#### 八，过滤器 	 

​	Vue.js 允许你自定义过滤器，可被用于一些常见的文本格式化。过滤器可以用在两个地方：**双花括号值**

​	**和 v-bind 表达式**。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符号指示： 

​	过滤器的定义语法：
		在html中 过滤器调用时候的格式	{{ item.name | 过滤器的名称 }} 
		在js中定义过滤器  Vue.filter('过滤器的名称'，function(){})
		函数里面的参数	第一个参数永远是需要过滤的数据，已经规定好了
						Vue.filter('过滤器的名称'，function(data){})
						return 过滤器的名称	

​	注意：	当有局部和全局两个名称相同的过滤器时候，会以就近原则进行调用，

​			即：**局部过滤器优先于全局过滤器被调用！**			

##### 私有过滤器 filters

​	1.HTML元素

```
<td>{{item.ctime | dataFormat()}}</td>
```

​	2.私有 filters 定义方式：

```
filters: { // 私有局部过滤器，只能在 当前 VM 对象所控制的 View 区域进行使用

    'dataFormat' function (dateStr) {
      // 根据给定的时间字符串，得到特定的时间
      var dt = new Date(dateStr);

      //   yyyy-mm-dd
      var y = dt.getFullYear();
      //先转字符串,再补全
      var m = (dt.getMonth() + 1).toString().padStart(2,'0');
      var d = dt.getDate().toString().padStart(2,'0');

      // 字符串补全: padStart()用于头部补全，padEnd()用于尾部补全。
      padStart(参数一，参数2)	 
      				参数一: 补全完毕之后字符串的总长度,
      				参数2,用来补全的字符串。

        var h = dt.getHours().toString().padStart(2,'0');
        var mm = dt.getMinutes().toString().padStart(2,'0');
        var s = dt.getSeconds().toString().padStart(2,'0');

        return `${y}-${m}-${d} ${h}:${mm}:${s}`
    }
  }
```

##### 全局过滤器 filter

​	3.全局 filter定义方式：

```
Vue.filter('dateFormat', function (dateStr) {
      // 根据给定的时间字符串，得到特定的时间
      var dt = new Date(dateStr);

      //   yyyy-mm-dd
      var y = dt.getFullYear();
      //先转字符串,再补全
      var m = (dt.getMonth() + 1).toString().padStart(2,'0');
      var d = dt.getDate().toString().padStart(2,'0');

      // 字符串补全: padStart()用于头部补全，padEnd()用于尾部补全。
      padStart(参数一，参数2)	 
      				参数一: 补全完毕之后字符串的总长度,
      				参数2,用来补全的字符串。

        var h = dt.getHours().toString().padStart(2,'0');
        var mm = dt.getMinutes().toString().padStart(2,'0');
        var s = dt.getSeconds().toString().padStart(2,'0');

        return `${y}-${m}-${d} ${h}:${mm}:${s}`

    });
```

#### 九，自定义指令

​	1.HTML元素

```
<input type="text" v-model="searchName" v-focus v-color="'red'" v-font-weight="900">
```

​	2.自定义全局指令和局部指令：

```
//自定义全局指令 v-focus，为绑定的元素自动获取焦点：
     Vue.directive('focus', {
        inserted: function (el) { // inserted 表示被绑定元素插入父节点时调用
            el.focus();
        }
    });

//自定义局部指令 v-color 和 v-font-weight，为绑定的元素设置指定的字体颜色 和 字体粗细：
      directives: {
          color: { // 为元素设置指定的字体颜色
             bind(el, binding) {
           		 el.style.color = binding.value;
         	 }
          },
          'font-weight': function (el, binding2) { 
        	 // 自定义指令的简写形式，等同于定义了 bind 和 update 两个钩子函数
          	 el.style.fontWeight = binding2.value;
          }
      }
```

```
		//定义全局的指令 v-focus
		//注意：调用时，必须要在指令名称加上v-前缀
		//		定义时 名称 ---> focus
		//参数1：指令的名称
		//参数2：是一个对象，在这个对象上有一些指令相关的函数，
		//			这些函数可以再特定的阶段执行相关的操作
		Vue.directive('focus',{
			bind:function (el) {//每当指令绑定到元素身上时，会立即执行这个bind函数，只执行一次
				el.focus();
				//元素刚绑定指令的时候，只是解析到内存当中了，还没有插入到DOM中
				//和颜色相关的可以在bind中执行
			},
			inserted:function (el) {//当元素插入到Dom中的时候，会执行inserted函数，只执行一次
				el.focus();
				//和js行为相关的操作，最好在inserted中执行，防止不生效。
			},
			update:function (el) {//当DOM节点更新的时候，会执行，会触发多次
				// body...
			}
		})
```

#### 十，键盘修饰符

**1.常见键盘修饰符**

```
<!-- 只有在 keyCode 是 13 时调用 vm.submit() -->
<input v-on:keyup.13="submit">
<input v-on:keyup.enter="submit">
//简写
<input @keyup.enter="submit">
```

全部的按键别名：

- enter
- tab
- delete
- esc
- space
- up
- down
- left
- right

**2.自定义键盘修饰符**

通过`Vue.config.keyCodes.名称 = 按键值`来自定义案件修饰符的别名：

```
Vue.config.keyCodes.f2 = 113;
```

使用自定义的按键修饰符：

```
<!-- 只有在 keyCode 是 113 时调用 vm.add() -->
<input type="text" v-model="name" @keyup.f2="add">
```

#### 十一，生命周期

- 什么是生命周期：从Vue实例创建、运行、到销毁期间，总是伴随着各种各样的事件，这些事件，统称为生命周期！
- [生命周期钩子](https://cn.vuejs.org/v2/api/#选项-生命周期钩子)：就是生命周期事件的别名而已；
- 生命周期钩子 = 生命周期函数 = 生命周期事件

1.html元素

```
<button @click = "msg='no'">修改msg</button>
<h1 id="h1"> {{msg}} </h1>
```

2.生命周期

```
	let vm = new Vue({
			el:'#app',
			data:{
				msg:'ok'
			},
			methods:{
				show(){
					console.log('执行了show方法')
				}
			},
			beforeCreate(){//第一个生命周期函数 表示在实例创建出来之前，会执行他
			 	console.log(this.msg);
			 	this.show();
				//说明：data和methods中的方法还没有初始化
			},
			created(){//第二个生命周期函数 
				console.log(this.msg);
				this.show();
				//说明：data和methods中的方法已经初始化
			},
			beforeMount(){//第三个生命周期函数 表示模板已经在内存当中编辑完了，尚未渲染在页面当中
				console.log(document.getElementById('h1').innerText);
				// this.show();
				//说明：在内存当中编辑完了，尚未渲染在页面当中
			},
			mounted(){//第四个生命周期函数 表示模板已经在渲染到页面当中
				console.log(document.getElementById('h1').innerText);
				// this.show();
				//说明：模板已经在渲染到页面当中
			},
			beforeUpdate(){//第五个生命周期函数
				console.log("页面上的内容"+document.getElementById('h1').innerText);
				console.log("内存data里面的内容"+this.msg)
				//说明：显示在页面当中的数据还未更新，而data的数据已经更新
			},
			updated(){//第六个生命周期函数
				console.log("页面上的内容"+document.getElementById('h1').innerText);
				console.log("内存data里面的内容"+this.msg)
				//说明：显示在页面当中的数据和data的数据已经同步更新
			},
			beforeDestroy(){//第七个生命周期函数，
				//实例销毁之前调用。在这一步，实例仍然完全可用。
			},
			destroyed(){//第八个生命周期函数，
				//Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，
						所有的事件监听器会被移除，所有的子实例也会被销毁。
			},
		})
```





