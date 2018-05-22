### 使用教程
 
 组件暴露：
 ```bash
	props:{
		dataList:[...],//需要渲染的列表
		liheight: {
		  default: 44
		}
	}
	method：selectedchange //自定义方法，传选中值给父组件
		
```	


组件内有部分注释，小伙伴可根据业务需求自行修改，
实际业务中列表数据应该是包含多个对象的数组，
demo中只渲染了字符串数组
```bash
	<li v-for="(item,i) in data" :style="{height:liheight+'px',lineHeight:liheight+'px'}" 
          :class="{'current-pre3':(index-3)===i || (index+3)===i,
            'current-pre2':(index-2)===i || (index+2)===i,
            'current-pre1':(index-1)===i || (index+1)===i,
            'current':index===i}">{{item}}</li>
	

```
大家根据业务场景自行修改所需显示的内容



### 在父组件中引入

```bash
	//template
	<div class="picker-box">
		
        <picker :dataList="list" liheight="50" @selectedchange="fn"></picker>
    </div>
	//js
	import picker from '../pick.vue' //引入
	export default {
		components: {
		  picker,
		},
		data(){
			list:[...]
		},
		methods:{
			fn(value){
				console.log(value);//获得选中值
			}
		}
		
	
	}
		
```	

样式：
[预览图](http://m.qpic.cn/psb?_blank/348b0b66-1122-4625-babe-eadc4afe8b15/JkJydOVZc1rjuZdXZwz8L8k3QO1i5Zji5q0It2J9qeg!/b/dGYBAAAAAAAA&bo=PwH6AD8B.gADFzI!&rf=viewer_4&t=5?_blank)


附带原生js版本