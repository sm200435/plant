<template>
  <div class="dashboard-container">
    <div class="dashboard-header">
      <div id="header-box1" ref="boxs1"></div>
      <div id="header-box2"></div>
      <div id="header-box3"></div>
      <div id="header-box4"></div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
import * as echarts from "echarts";
export default {
  name: "Dashboard",
  computed: {
    ...mapGetters(["name"]),
  },
  data(){
    return{
    }
  },
  methods: {
    headerBox1() {
      var myChart = echarts.init(document.getElementById("header-box1"));
      var TP_value = 20.3;
    var kd = [];
    var Gradient = [];
    var leftColor = '';
    var showValue = '';
    var boxPosition = [65, 0];
    var TP_txt = ''
    // 刻度使用柱状图模拟，短设置1，长的设置3；构造一个数据
    for (let i = 0, len = 60; i <= len; i += 1) {
        if (i < 10) {
            kd.push('');
        } else if ((i - 10) % 10 === 0) {
            kd.push('-3');
        } else {
            kd.push('-1');
        }
    }
    //中间线的渐变色和文本内容
    if(TP_value > 24) {
        TP_txt = '温度偏高';
        Gradient.push({
            offset: 0,
            color: '#93FE94'
        }, {
            offset: 0.5,
            color: '#E4D225'
        }, {
            offset: 1,
            color: '#E01F28'
        })
    } else if(TP_value >= 14) {
        TP_txt = '温度正常';
        Gradient.push({
            offset: 0,
            color: '#93FE94'
        }, {
            offset: 1,
            color: '#E4D225'
        })
    } else {
        TP_txt = '温度偏低';
        Gradient.push({
            offset: 1,
            color: '#93FE94'
        })
    }
    if(TP_value > 50) {
        showValue = 50
    } else {
        if(TP_value < 0) {
            showValue = 0
        } else {
            showValue = TP_value + 10
        }
    }
    if(TP_value < 0) {
        boxPosition = [65, -120];
    }
    leftColor = Gradient[Gradient.length - 1].color;
    // 因为柱状初始化为0，温度存在负值，所以加上负值60和空出距离10
    var option = {
        grid:{
            x: 30,   //左侧与y轴的距离
            y: -130,   //top部与x轴的距离
            x2: -120,   //右侧与y轴的距离
            y2: 20    //bottom部与x轴的距离
        },
        backgroundColor: '#0C2F6F',
        title: {
            text: '温度计',
            show: false
        },
        yAxis: [{
            show: false,
            data: [],
            min: 0,
            max: 120,
            axisLine: {
                show: false
            }
        }, {
            show: false,
            min: 0,
            max: 60,
        }],
        xAxis: [{
            show: false,
            min: -10,
            max: 80,
            data: []
        }, {
            show: false,
            min: -10,
            max: 80,
            data: []
        }, {
            show: false,
            min: -10,
            max: 80,
            data: []
        }, {
            show: false,
            min: -5,
            max: 80,

        }],
        series: [{
            name: '条',
            type: 'bar',
            // 对应上面XAxis的第一个对象配置
            xAxisIndex: 0,
            data: [{
                value: showValue
            }],
            barWidth: 8,
            itemStyle: {
                normal: {
                    color: new echarts.graphic.LinearGradient(0, 1, 0, 0, Gradient)
                }
            },
            z: 2
        }, {
            name: '白框',
            type: 'bar',
            xAxisIndex: 1,
            barGap: '-100%',
            data: [60],
            barWidth: 14,
            itemStyle: {
                normal: {
                    color: '#0C2E6D',
                    barBorderRadius: 50,
                }
            },
            z: 1
        }, {
            name: '外框',
            type: 'bar',
            xAxisIndex: 2,
            barGap: '-100%',
            data: [61],
            barWidth: 24,
            itemStyle: {
                normal: {
                    color: '#4577BA',
                    barBorderRadius: 50,
                }
            },
            z: 0
        }, {
            name: '圆',
            type: 'scatter',
            hoverAnimation: false,
            data: [0],
            xAxisIndex: 0,
            symbolSize: 20,
            itemStyle: {
                normal: {
                    color: '#93FE94',
                    opacity: 1,
                }
            },
            z: 2
        }, {
            name: '白圆',
            type: 'scatter',
            hoverAnimation: false,
            data: [0],
            xAxisIndex: 1,
            symbolSize: 24,
            itemStyle: {
                normal: {
                    color: '#0C2E6D',
                    opacity: 1,
                }
            },
            z: 1
        }, {
            name: '外圆',
            type: 'scatter',
            hoverAnimation: false,
            data: [0],
            xAxisIndex: 2,
            symbolSize: 30,
            itemStyle: {
                normal: {
                    color: '#4577BA',
                    opacity: 1,
                }
            },
            z: 0
        }, {
            name: '刻度',
            type: 'bar',
            yAxisIndex: 0,
            xAxisIndex: 3,
            label: {
                normal: {
                    show: true,
                    position: 'left',
                    distance: 10,
                    color: 'white',
                    fontSize: 14,
                    formatter: function(params) {
                        if (params.dataIndex > 60 || params.dataIndex < 10) {
                            return '';
                        } else {
                            if ((params.dataIndex - 10) % 10 === 0) {
                                return params.dataIndex - 10;
                            } else {
                                return '';
                            }
                        }
                    }
                }
            },
            barGap: '-100%',
            data: kd,
            barWidth: 1,
            itemStyle: {
                normal: {
                    color: 'white',
                    barBorderRadius: 120,
                }
            },
            z: 0
        }]
    };
      myChart.setOption(option)
      window.onresize=function(){
      myChart.resize()
      }
    },
  },
  created () {

},
mounted () {
    this.headerBox1()
},
};
</script>

<style lang="scss" scoped>
.dashboard-container {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  padding: 30px;
  .dashboard-header {
    width: 100%;
    height: 150px;
    display: flex;
    justify-content: space-between;
    div {
      width: calc(100% / 4 - 15px);
    }
  }
}
</style>
