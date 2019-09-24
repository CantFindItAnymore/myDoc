### 一. 获取

`npm install echarts --save`

`npm i -s echarts-for-react`

### 二. 使用示例

```javascript
import React, { Component } from 'react'
import ReactEcharts from 'echarts-for-react'

export default class ECharts extends Component {
  render() {
    const option = {
      xAxis: {
          type: 'category',
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
      },
      yAxis: {
          type: 'value'
      },
      series: [{
          data: [820, 932, 901, 934, 1290, 1330, 1320],
          type: 'line'
      }]
    }
　　return (
　　　<ReactEcharts  option={option} style={{height:'200px',width:'100%'}} />
　　)
  }
}
```

