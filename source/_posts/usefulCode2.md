---
title: 有用的代码段-ts篇
date: 2020-01-16 19:34:10
tags: 
- tips
categories: 
- 工作经验
---
# 查询数组中值相等的
```typescript
modules.modules.find(item => item.route === route).chinese

```
# ref调用子组件
```typescript
<FacePointsMarkTool ref={com => this.com = com} customRightRender={this.customRightRender} hideSaved={this.handleHideSaved} dataSource={dataSource} loadMore={this.getFaceData} isShowFacePlus saveFaceData={this.saveFaceData}></FacePointsMarkTool>
```
