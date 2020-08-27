# vue-simple-tab-switch


A simple vue dynamic switch tab


### Notice
Not support IE 不支持IE浏览器

### Example

<img src="https://s1.ax1x.com/2020/08/27/dfvzFI.png" width="300">

### Demo
Run `npm run dev` or `yarn dev`  

运行 `npm run dev` or `yarn dev`

## Installation
**install with NPM**
```bash
npm install vue-simple-tab-switch --save
```
**Import**
```js
import VueTabSwitch from 'vue-simple-tab-switch'

new Vue({
    components: {VueTabSwitch}
})
```
## Usage
**In template**

```html
  <vue-tab-switch 
    :tabs="datas2">
  </vue-tab-switch>
  
  <vue-tab-switch
    :tabs="datas" 
    :textActiveColor="activeColor"
    :textColor="color"
    :lineColor="activeColor"
    @changeTab="changeTab">
  </vue-tab-switch>
```

```js
export default {
  data() {
    return {
      activeColor: '#409eff',
      color: '#c0c4cc',
      datas: [
        {text: '推荐推荐', value: 1},{text: '军事', value: 1},         {text: '历史', value: 1},{text: '教育', value: 1},
        {text: '推荐', value: 1},{text: '军事军事', value: 1},{text: '历史', value: 1},{text: '教育', value: 1},
        {text: '推荐', value: 1},{text: '军事', value: 1},{text: '历史历史', value: 1},{text: '教育', value: 1},
        {text: '推荐', value: 1},{text: '军事', value: 1},{text: '历史', value: 1},{text: '教育', value: 1},
      ],
      datas2: [
        {text: '新闻', value: 1},{text: '教育', value: 1},{text: '历史', value: 1},{text: '科学', value: 1}
      ]
    };
  },
  methods:{
    changeTab : function(res) {
      console.log(res);
    }
  }
}
```
Parameter | Explanation
----|----
tabs | tab data 导航栏数据集
textActiveColor|  actived text color 导航栏选中和触发高亮色
textColor | default text color 导航栏默认颜色
lineColor | line color 下标线的颜色

