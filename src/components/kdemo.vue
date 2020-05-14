<template>
  <div class="kdemo-page">
    <div id="echartContainer" ref="echartContainer"></div>
  </div>
</template>

<script>
/* eslint-disable */

export default {
  name: 'kdemo',
  data() {
    return { // 数据模型
      //模型 time      open    close    lowest  highest     vol    macd    dif    dea
      //['2015-10-19', 18.56,  18.25,   18.19,  18.56,     55.00,  -0.00,  0.08,  0.09]
      data: [
        ["2019-3-16", 18.4, 18.58, 18.33, 18.79, 67.0, 0.04, 0.11, 0.09],
        ["2019-3-19", 18.56, 18.25, 18.19, 18.56, 55.0, -0.0, 0.08, 0.09],
        ["2019-3-20", 18.3, 18.22, 18.05, 18.41, 37.0, 0.01, 0.09, 0.09],
        ["2019-3-21", 18.18, 18.69, 18.02, 18.98, 89.0, 0.03, 0.1, 0.08],
        ["2019-3-22", 18.42, 18.29, 18.22, 18.48, 43.0, -0.06, 0.05, 0.08],
        ["2019-3-23", 18.26, 18.19, 18.08, 18.36, 46.0, -0.1, 0.03, 0.09],
        ["2019-3-26", 18.33, 18.07, 17.98, 18.35, 65.0, -0.15, 0.03, 0.1],
        ["2019-3-27", 18.08, 18.04, 17.88, 18.13, 37.0, -0.19, 0.03, 0.12],
        ["2019-3-28", 17.96, 17.86, 17.82, 17.99, 35.0, -0.24, 0.03, 0.15],
        ["2019-3-29", 17.85, 17.81, 17.8, 17.93, 27.0, -0.24, 0.06, 0.18],
        ["2019-3-30", 17.79, 17.93, 17.78, 18.08, 43.0, -0.22, 0.11, 0.22],
        ["2019-4-02", 17.78, 17.83, 17.78, 18.04, 27.0, -0.2, 0.15, 0.25],
        ["2019-4-03", 17.84, 17.9, 17.84, 18.06, 34.0, -0.12, 0.22, 0.28],
        ["2019-4-04", 17.97, 18.36, 17.85, 18.39, 62.0, -0.0, 0.3, 0.3],
        ["2019-4-05", 18.3, 18.57, 18.18, 19.08, 177.0, 0.07, 0.33, 0.3],
        ["2019-4-06", 18.53, 18.68, 18.3, 18.71, 95.0, 0.12, 0.35, 0.29],
        ["2019-4-09", 18.75, 19.08, 18.75, 19.98, 202.0, 0.16, 0.35, 0.27],
        ["2019-4-10", 18.85, 18.64, 18.56, 18.99, 85.0, 0.09, 0.29, 0.25],
        ["2019-4-11", 18.64, 18.44, 18.31, 18.64, 50.0, 0.06, 0.27, 0.23],
        ["2019-4-12", 18.55, 18.27, 18.17, 18.57, 43.0, 0.05, 0.25, 0.23],
        ["2019-4-13", 18.13, 18.14, 18.09, 18.34, 35.0, 0.05, 0.24, 0.22],
        ["2019-4-16", 18.01, 18.1, 17.93, 18.17, 34.0, 0.07, 0.25, 0.21],
        ["2019-4-17", 18.2, 18.14, 18.08, 18.45, 58.0, 0.11, 0.25, 0.2],
        ["2019-4-18", 18.23, 18.16, 18.0, 18.45, 47.0, 0.13, 0.25, 0.19],
        ["2019-4-19", 18.08, 18.2, 18.05, 18.25, 32.0, 0.15, 0.24, 0.17],
        ["2019-4-20", 18.15, 18.15, 18.11, 18.29, 36.0, 0.13, 0.21, 0.15],
        ["2019-4-23", 18.16, 18.19, 18.12, 18.34, 47.0, 0.11, 0.18, 0.13],
        ["2019-4-24", 18.23, 17.88, 17.7, 18.23, 62.0, 0.03, 0.13, 0.11],
        ["2019-4-25", 17.85, 17.73, 17.56, 17.85, 66.0, -0.03, 0.09, 0.11],
        ["2019-4-26", 17.79, 17.53, 17.5, 17.92, 63.0, -0.1, 0.06, 0.11],
        ["2019-4-27", 17.51, 17.04, 16.9, 17.51, 67.0, -0.16, 0.05, 0.13],
        ["2019-4-30", 17.07, 17.2, 16.98, 17.32, 55.0, -0.12, 0.09, 0.15]
      ],
      data0: "", // 处理过的数据
      kColor: {
        up: '#e32350',
        down: '#1db9bb'
      },
      maColor: {
        ma5: '#4698cd',
        ma10: '#ead15e',
        ma20: '#d84799',
        ma30: '#2f6e72'
      },
      macdColor: {
        MACD: '',
        dea: '#7b0f2f',
        dif: '#0f59af'
      },
    }
  },
  mounted() {
    this.data0 = this.splitData(this.data);
    this.drawLine(this.data0);
  },
  methods: {
    /*数据初步处理*/
    splitData: function(rawData) {
      var categoryData = [];    // 日期 
      var values = [];          // 除去日期的数组集合
      var vol = [];             // 成交量     
      var macds = [];           // macd
      var difs = [];            // dif
      var deas = [];            // dea
      for (var i=0; i<rawData.length; i++) {
        categoryData.push(rawData[i].splice(0, 1)[0]);
        values.push(rawData[i]);
        vol.push([i, rawData[i][4], rawData[i][0] > rawData[i][1] ? 1 : -1]);
        macds.push([i, rawData[i][5], rawData[i][5] < 0 ? 1 : -1]);
        difs.push(rawData[i][6]);
        deas.push(rawData[i][7]);
      }
      return {
        categoryData: categoryData,
        values: values,
        vol: vol,
        macds: macds,
        difs: difs,
        deas: deas
      };
    },

    /*ma 均线函数:
    * 计算公式：dayCount 天的收盘价总和 / dayCount
    */ 
    calculateMA: function(dayCount) {
      var result = [];
      for (var i = 0, len = this.data0.values.length; i < len; i++) {
        if (i < dayCount) {
          result.push("-");
          continue;
        }
        var sum = 0;
        for (var j = 0; j < dayCount; j++) {
          sum += this.data0.values[i - j][1];
        }
        result.push(sum / dayCount);
      }
      return result;
    },
    
    /* 指定图表的配置项和数据
      1、tooltip: 提示框组件
        trigger：触发类型 axis(坐标轴触发，主要在柱状图，折线图等会使用类目轴的图表中使用)
        axisPointer: 坐标轴指示器配置项
      2、grid：直角坐标系内绘图网格
            (width、height 设置组件大小; left、right、top、bottom 设置组件位置;)
      3、xAxis： 直角坐标系 grid 中的 x 轴
            type: 坐标轴类型(默认类目轴: category) 
            data: 类目数据，数组形式
            axisLine:  坐标轴轴线相关设置  
            axisTick： 坐标轴刻度相关设置
            axisLabel: 坐标轴刻度标签的相关设置 
      4、yAxis： 直角坐标系 grid 中的 y 轴
            splitLine：坐标轴在 grid 区域中的分隔线
            splitArea: 坐标轴在 grid 区域中的分隔区域
            position:  轴的位置
      5、series：画线
          type: 画线类型(candlestick: K线图)  
          data：数据 [open, close, lowest, highest] 即：[开盘值, 收盘值, 最低值, 最高值]
          type: 画线类型(line: 折线图)  
          symbol: 标记的图形
    */ 
    drawLine: function(data0) {
      var option = {
        tooltip: {    
          trigger: "axis",
          axisPointer: {
            type: "cross"
          }
        },
        visualMap: {
          show: false,
          seriesIndex: [5,6],
          dimension: 2,
          pieces: [{
              value: 1,
              color: this.kColor.down
          }, {
              value: -1,
              color: this.kColor.up
          }]
        },
        grid: [
          {                                 // K线 区域设置
            top: '2%',
            left: '8%',
            right: '8%',
            height: '52%'                  // 410 / 790
          },
          {                                 // 成交量 区域设置          
            left: "8%",
            right: "8%",
            top: '54%',
            height: "24%"                  // 147 / 790  
          },
          {                                 // macd 区域设置          
            left: "8%",
            right: "8%",
            top: '76%',
            bottom: '2%',
            height: "24%"                 // 143 / 790  
          }
        ],
        xAxis: [
          {                               // x 轴             
            type: "category",
            data: data0.categoryData,
            axisTick: {
              show: false
            },
            axisLabel: {
              show: false
            }
          },
          {
            type: "category",
            gridIndex: 1,
            data: data0.categoryData,
            axisLabel: {
              show: false
            },
            axisTick: {
              show: false
            }
          },
          {
            type: "category",
            gridIndex: 2,
            data: data0.categoryData,
            axisLine: {
              lineStyle: {
                color: "#243966"
              }
            },
            axisTick: {
              show: false
            }
          }
        ],
        yAxis: [                               // y 轴 
          {          
            scale: true,
            axisLine: {
              show: false,
              lineStyle: {
                color: "red"
              }
            },
            axisTick: {
              show: false
            },
            axisLabel: {
              inside: true,
              margin: 0
            },
            splitLine: {
              lineStyle: {
                color: ['#243966']
              }
            },
            splitArea: {
              show: true,
              areaStyle:{
                color:['#08112c']
              }
            }
          },
          {
            gridIndex: 1,
            splitNumber: 2,
            axisLine: { show: false },
            axisTick: { show: false },
            axisLabel: { show: false },
            splitLine: {
              lineStyle: {
                color: ['#243966']
              }
            },
            splitArea: {
              show: true,
              areaStyle:{
                color:['#08112c']
              }
            }
          },
          {
            gridIndex: 2,
            splitNumber: 2,
            axisLine: {
              show: false,
              lineStyle: {
                color: "red"
              }
            },
            axisTick: {
              show: false
            },
            axisLabel: {
              inside: true,
              margin: 0
            },
            splitLine: {
              lineStyle: {
                color: ['#243966']
              }
            },
            splitArea: {
              show: true,
              areaStyle:{
                color:['#08112c']
              }
            }
          }
        ],
        series: [
          {                                    // K线
            type: "candlestick",
            data: data0.values,
            itemStyle: {
              color: this.kColor.up,     
              color0: this.kColor.down,
              borderColor: null,
              borderColor0: null,
            }
          },
          {                                   // ma5线              
            name: "MA5",
            type: "line",
            data: this.calculateMA(5),
            symbol: "none",
            lineStyle: {
              color: this.maColor.ma5,
              opacity: 0.5
            }
          },
          {                                    // ma10线   
            name: "MA10",
            type: "line",
            data: this.calculateMA(10),
            symbol: "none",
            lineStyle: {
              color: this.maColor.ma10,
              opacity: 0.5
            }
          },
          {                                    // ma20线   
            name: "MA20",
            type: "line",
            data: this.calculateMA(20),
            symbol: "none",
            lineStyle: {
              color: this.maColor.ma20,
              opacity: 0.5
            }
          },
          {                                     // ma30线   
            name: "MA30",
            type: "line",
            data: this.calculateMA(30),
            symbol: "none",
            lineStyle: {
              color: this.maColor.ma30,
              opacity: 0.5
            }
          },
          {                                     // 成交量
            name: 'Volume',
            type: 'bar',
            xAxisIndex: 1,
            yAxisIndex: 1,
            data: data0.vol,
          },
          {                                     // MACD   
            name: "MACD",
            type: "bar",
            xAxisIndex: 2,
            yAxisIndex: 2,
            data: data0.macds,
            barWidth: 1
          },
          {
            name: "DIF",
            type: "line",
            xAxisIndex: 2,
            yAxisIndex: 2,
            data: data0.difs,
            symbol: "none",
            lineStyle: {
              color: this.macdColor.dif,
              opacity: 0.5
            }
          },
          {
            name: "DEA",
            type: "line",
            xAxisIndex: 2,
            yAxisIndex: 2,
            data: data0.deas,
            symbol: "none",
            lineStyle: {
              color: this.macdColor.dea,
              opacity: 0.5
            }
          }
        ]
      };
      // 初始化
      var charts = this.$echarts.init(this.$refs.echartContainer);
      charts.setOption(option);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #echartContainer{
    width:100%; 
    height: 10.5333rem /* 790/75 */;
  }
</style>
