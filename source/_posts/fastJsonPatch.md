---
title: fast-json-patch
date: 2019-07-26 14:37:10
tags: 小工具
categories: 
- 前端
- react
- json-patch
---
# fast-json-patch包使用
网上关于json-patch的中文介绍太少了，官方文档没有翻译版，所以写一下自己的学习文档，以下只是自己根据英文文档的理解，不保证正确。
## 基础操作
json patch几大基础操作包括：add, replace, move, test, remove, copy
## applyPatch
给定文档和操作，执行操作
```typescript
import {applyOperation, applyPatch} from 'fast-json-patch';
let document = document = { firstName: "Albert", contactDetails: { phoneNumbers: [] } };
let patch = [
  { op: "replace", path: "/firstName", value: "Joachim" },
  { op: "add", path: "/lastName", value: "Wester" },
  { op: "add", path: "/contactDetails/phoneNumbers/0", value: { number: "555-123" }  }
];
const docu = applyPatch(document, patch)
```
## applyOperation
执行单独的操作而不是连贯操作,举例如下：
```typescript
var document = { firstName: "Albert", contactDetails: { phoneNumbers: [] } };
var operation = { op: "replace", path: "/firstName", value: "Joachim" };
document = jsonpatch.applyOperation(document, operation).newDocument;
// document == { firstName: "Joachim", contactDetails: { phoneNumbers: [] }}
```
## applyReducer
看了英文原文文档并不很确定，只能根据自己的理解来看。    
javascript的reduce是一个累加器，使用applyReducer作为累加操作，当定义的操作是一组数组时，使用applyReducer执行操作，示例如下：
```typescript
var document = { firstName: "Albert", contactDetails: { phoneNumbers: [ ] } };
var patch = [
  { op:"replace", path: "/firstName", value: "Joachim" },
  { op:"add", path: "/lastName", value: "Wester" },
  { op:"add", path: "/contactDetails/phoneNumbers/0", value: { number: "555-123" } }
];
var updatedDocument = patch.reduce(applyReducer, document);
// updatedDocument == { firstName:"Joachim", lastName:"Wester", contactDetails:{ phoneNumbers[ {number:"555-123"} ] } };
```
## Generating patches
提供一个类似于观察者，先获取原始文档结构（observe），之后文档进行变化，最后执行操作，可以得到应有的操作流程。示例如下：
```typescript
var document = { firstName: "Joachim", lastName: "Wester", contactDetails: { phoneNumbers: [ { number:"555-123" }] } };
var observer = jsonpatch.observe(document);
document.firstName = "Albert";
document.contactDetails.phoneNumbers[0].number = "123";
document.contactDetails.phoneNumbers.push({ number:"456" });
var patch = jsonpatch.generate(observer);
// patch  == [
//   { op: "replace", path: "/firstName", value: "Albert"},
//   { op: "replace", path: "/contactDetails/phoneNumbers/0/number", value: "123" },
//   { op: "add", path: "/contactDetails/phoneNumbers/1", value: {number:"456"}}
// ];
```
## 比较两个文档-compare
直接比较两个文档，得出从文档A到文档B应有哪些操作（得到操作后可以尝试用这个操作来更新B文档）
```typescript
var documentA = {user: {firstName: "Albert", lastName: "Einstein"}};
var documentB = {user: {firstName: "Albert", lastName: "Collins"}};
var diff = jsonpatch.compare(documentA, documentB);
//diff == [{op: "replace", path: "/user/lastName", value: "Collins"}]
```
## 验证修补程序序列
对patch做校验，验证这个补丁是否可以应用到指定的对象上（可能是这个意思）,如果出错可以打印出错误信息
```typescript
var obj = {user: {firstName: "Albert"}};
var patches = [{op: "replace", path: "/user/firstName", value: "Albert"}, {op: "replace", path: "/user/lastName", value: "Einstein"}];
var errors = jsonpatch.validate(patches, obj);
if (errors.length == 0) {
 //there are no errors!
}
else {
  for (var i=0; i < errors.length; i++) {
    if (!errors[i]) {
      console.log("Valid patch at index", i, patches[i]);
    }
    else {
      console.error("Invalid patch at index", i, errors[i], patches[i]);
    }
  }
}
```
# Json Patch API
函数原型：
```typescript
function applyPatch<T>(document: T, patch: Operation[], validateOperation?: boolean | Validator<T>, mutateDocument: boolean = true, banPrototypeModifications: boolean = true): PatchResult<T>
```
对参数的释义：    
* document：将要patch的文档；    
* patch：一个json-patch数组，一组可执行的操作；    
* validateOperation： boolean类型，是否使用默认校验器对每个操作进行校验或通过校验器回调；    
...
其余部分可以自行查文档
