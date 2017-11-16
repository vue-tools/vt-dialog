# vt-dialog
vue弹窗组件  
 
# Use 
```shell
$ npm install vt-dialog --save
```

```shell
 import Dialog from 'vt-dialog'
 
 Vue.component('Dialog', Dialog)
```
  
# Api 
 
### Props 
*   `title` String类型，弹框的标题，默认不显示
*   `opened` bool类型，控制弹窗显示隐藏的字段，默认 `false` 
*   `closable` bool类型，控制弹窗右上角是否显示关闭按钮，默认 `true`
*   `moveable` bool类型，控制弹窗是否可以拖动，默认 `true` 

### Eent 
*   `close` 当弹窗关闭时所触发的事件

### Slot 
*  `default` 弹窗中嵌套的内容卡槽
*  `footer` 弹窗底部嵌套的页脚卡槽，一般用来放置操作按钮，非必须