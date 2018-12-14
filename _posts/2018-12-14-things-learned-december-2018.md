---
theme: post
published: true
title: Things learned – December 2018
---
# December 14

### Emulate `expandAll` property in antd `<Table />`

```jsx
<Table
  dataSource={someData}
  rowKey="definingKey"
  expandIcon={() => null} // must be a function
  expandedRowKeys={someData.map(g => g.definingKey)}
  expandRowByClick={false} // to disable hover
  // ...
/>
```