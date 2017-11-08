---
title: Vue 和 Echarts
date: 2017-07-21 14:47:23
tags: [vue,echarts]
categories: [学习]
---

讲讲在 Vue 使用 Echarts.

<!--more-->

## 首先安装依赖包:
在项目顶级目录下执行如下命令:
```
npm install echarts -S
```

echarts 和 axios 类似,没法通过 Vue.use()全局配置.
通常在需要使用 echarts 图表的 .vue 文件中直接引入.
```js
import echarts from 'echarts'
```

觉得太麻烦的话,可以在 main.js 中引用,然后修改原型链,就可以全局使用了.
```js
Vue.prototype.$echarts = echarts
```

## 创建图表
1. 首先在 HTML 模板中创建图表的容器
```html
<div id="chart"></div>
</div>
```

2. 为容器设置宽高(注意:这里不能用百分比,可以用 JS 控制并且改变width 和 height)
```scss
#chart{
    width:7.5rem;
    height:6rem;
}
```

3. 图表初始化
因为 echarts.init 方法必须绑定到实际的 DOM 元素上,所以该方法需要在vue 生命周期 mounted里调用:
```
let chart = this.$echarts.init(docueng.getElementById('chart'))
chart.setOption(options);
```

```js

this.$nextTick(function(){
    //DOM 更新
    });
```

```js
export default{
        name: 'Line',
        props: {}, // 父到子传参
        components: {}, // 组件接收
        mixins: [], // 混合
        data () { // 基础数据
            return {
                //初始化空对象
                chart: null,
                colorArr: ['rgba(205,47,95,.5)', 'rgba(250,157,236,.5)', 'rgba(136,243,226,.5)', 'rgba(102,205,170,.5)'],
                legendArr: [],
                options: {
                    title: {
                        show: true,
                        text: ''
                    },
                    tooltip: {
                        trigger: 'axis',
                        axisPointer: {
                            type: 'cross',
                            label: {
                                backgroundColor: '#6a7985'
                            }
                        }
                    },
                    legend: {
                        data: ['邮件营销', '联盟广告', '视频广告', '直接访问', '搜索引擎']
                    },
                    toolbox: {
                        feature: {
                            saveAsImage: {}
                        }
                    },
                    grid: {
                        left: '5%',
                        right: '5%',
                        bottom: '3%',
                        containLabel: true
                    },
                    xAxis: [
                        {
                            type: 'category',
                            boundaryGap: false,
                            data: ['1','2','3','4','5','6']
                        }
                    ],
                    yAxis: [
                        {
                            type: 'value'
                        }
                    ],
                    series: [
                        {
                            name: '邮件营销',
                            type: 'line',
                            stack: '总量',
                            symbol: 'diamond',
                            symbolSize: 5,
                            areaStyle: {normal: {}},
                            data: [120, 132, 101, 134, 90, 230, 210]
                        },
                        {
                            name: '联盟广告',
                            type: 'line',
                            stack: '总量',
                            areaStyle: {normal: {}},
                            data: [220, 182, 191, 234, 290, 330, 310]
                        },
                        {
                            name: '视频广告',
                            type: 'line',
                            stack: '总量',
                            areaStyle: {normal: {}},
                            data: [150, 232, 201, 154, 190, 330, 410]
                        },
                        {
                            name: '直接访问',
                            type: 'line',
                            stack: '总量',
                            areaStyle: {normal: {}},
                            data: [320, 332, 301, 334, 390, 330, 320]
                        },
                        {
                            name: '搜索引擎',
                            type: 'line',
                            stack: '总量',
                            label: {
                                normal: {
                                    show: true,
                                    position: 'top'
                                }
                            },
                            areaStyle: {normal: {}},
                            data: [820, 932, 901, 934, 1290, 1330, 1320]
                        }
                    ],
                    color: ['rgba(205,47,95,.5)', 'rgba(250,157,236,.5)', 'rgba(0,191,255,.5)','rgba(102,205,170,.5)','rgba(72,61,139,.5)']
                },
            }
        },
        mounted(){
            this.$nextTick(function () {
                this.drawChart('LineChart')
            });
        },
        created () {
            // 创建周期
        },
        watch: {
            options: {
                handler(options) {
                    this.chart.setOption(this.options)
                },
                deep: true
            }
        },
        methods: {
            // 方法
            drawChart: function (id) {
                var self = this;
                this.chart = this.$echarts.init(document.getElementById(id));
                this.chart.setOption(self.options);
            }
        },
        computed: {}, // 计算属性
        filters: {}, // 过滤
        directives: {} // 指令
    }
```
