---
title: demo实例
date: 2019-07-18 09:37:10
tags: demo
---
## js正则表达式
```javascript
let patt = /[顺丰]/;
      let reg = new RegExp(patt, 'i');
      const company = (reg.test(record.remark) === true) ? 'shunfeng' : 'annengwuliu'
      console.log('re test-=-----', company);
      const num = record.remark.replace(/[^0-9]/ig, "")
```
## 递归读取层级json
```javascript
getAllKey (obj: any, name: string, moduleName?: string): any[] {
  const arr: any[] = []
    if (Object.keys(obj)) {
      let keys = Object.keys(obj);
      let i = 0;
      return keys.map((items) => {
        i++;
        if (obj[items] instanceof Object) {
          // console.log("obj[items]:", items);
          let itemRes = items;
          if (obj[items]['type']) {
            // console.log('it is number:', obj[items]['type']);
            itemRes = obj[items]['type'];
          }
          const names = name + '-' + itemRes
          return this.getAllKey(obj[items], names, moduleName);
        } else {
          // console.log('items:', name + '-' + items);
          const resItems = name + '-' + items;
          // console.log('split array:', resItems.split('-'));
          if (resItems == moduleName) {
            return this.handleRender(resItems, treeRule.modules, moduleName, i);
          }
        }
      });
    }
console.log('arr', arr)
    return arr
}
```
