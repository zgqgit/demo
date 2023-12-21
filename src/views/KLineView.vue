<template>
  <div class="k-line-box" ref="k-line-box" id="k-line-box">
    {{ title }}
  </div>
</template>

<script lang="ts">
import { ref, defineComponent, onMounted } from 'vue'
import echarts from '../plugins/echartsPlugin'
import chartData from '../apis/chartData'

export default defineComponent({
  setup() {
    const title = ref('这是kLine区')

    const upColor = '#ec0000'
    const upBorderColor = '#8A0000'
    const downColor = '#00da3c'
    const downBorderColor = '#008F28'

    onMounted(() => {
      initCharts()
    })
    const initCharts = () => {
      var myChart = echarts.init(document.getElementById('k-line-box'))
      myChart.setOption({
        title: {
          text: '上证指数',
          left: 0
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross'
          },
        },
        legend: {
          data: ['日K']
        },
        grid: {
          left: '10%',
          right: '10%',
          bottom: '15%'
        },
        xAxis: {
          type: 'category',
          data: chartData.xData,
          boundaryGap: false,
          axisLine: { onZero: false },
          splitLine: { show: false },
          min: 'dataMin',
          max: 'dataMax'
        },
        yAxis: {
          scale: true,
          splitArea: {
            show: true
          }
        },
        dataZoom: [
          {
            type: 'inside',
            start: 50,
            end: 100
          },
          {
            show: true,
            type: 'slider',
            top: '90%',
            start: 50,
            end: 100
          }
        ],
        series: [
          {
            name: '日K',
            type: 'candlestick',
            data: chartData.value,
            itemStyle: {
              color: upColor,
              color0: downColor,
              borderColor: upBorderColor,
              borderColor0: downBorderColor
            },
            markPoint: {
              symbol: 'pin',
              label: {
                formatter: function (param: any) {
                  return param != null ? Math.round(param.value) + '' : ''
                }
              },
              data: [
                {
                  name: '最大值',
                  type: 'max',
                  valueDim: 'highest'
                },
                {
                  name: '最小值',
                  type: 'min',
                  valueDim: 'lowest'
                },
                {
                  name: '收盘平均值',
                  type: 'average',
                  valueDim: 'close'
                }
              ],
              tooltip: {
                formatter: function (param: any) {
                  return param.name + '<br>' + (param.data.coord || '')
                }
              }
            },
            markLine: {
              symbol: ['none', 'none'],
              data: [
                {
                  name: '收盘最小值',
                  type: 'min',
                  valueDim: 'close'
                },
                {
                  name: '收盘最大值',
                  type: 'max',
                  valueDim: 'close'
                }
              ]
            }
          }
        ]
      })
    }

    return { title }
  }
})
</script>

<style scoped>
#k-line-box {
  width: 800px;
  height: 600px;
}
</style>
