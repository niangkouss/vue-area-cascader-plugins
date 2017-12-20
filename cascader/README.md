### Cascader级联组件

#### 使用
1.在main.js中引入
```javascript
import Cascader from './plugins/cascader'
Vue.use(Cascader);
```
2.结构
```html
<cascader :datas="datas"  @change="handlerChange" v-model="values" ></cascader>
```
- datas为级联组件的数据,类型为数组,必填
类型如下:
```javascript
datas:[
				{
    					value: 'ziyuan',
    					label: '资源',
    					children: [{
    						value: 'axure',
    						label: 'Axure Components'
    					}, {
    						value: 'sketch',
    						label: 'Sketch Templates'
    					}, {
    						value: 'jiaohu',
    						label: '组件交互文档'
    					}]
    			},
    			{
                    	value: 'zhinan',
                    	label: '指南',
                        children: [{
                    		value: 'shejiyuanze',
                    		label: '设计原则',
                    		children: [{
                    				value: 'yizhi',
                    				label: '一致'
                    			}, {
                    				value: 'fankui',
                    				label: '反馈'
                    			}, {
                    				value: 'xiaolv',
                    				label: '效率'
                    			}, {
                    				value: 'kekong',
                    				label: '可控'
                    				}]
                  }
    				
	
]
```

- @change 为当绑定值变化时触发的事件,可以通过arguments[0]为得到变化的值,得到的是选择的数组中的value字符串,如"zhian/shejiyuanze/yizhi"
- values双向绑定值,可以设置默认值,选择结束后可以得到数组的label值,如"指南/设计原则/一致"
- 备注   
- 下拉框默认是hover事件的时候显示,也可以替换成click才触发下拉框的显示,触发条件为 :trigger="click"使用:
```html
<cascader :datas="datas"  @change="handlerChange" v-model="values" :trigger="click"></cascader>
```
输入框的长度默认是100%,建议使用的时候在cascader组件外添加一个div,通过控制div的宽度来获得想要的组件宽度,cascader会自动继承这个div的宽度的



