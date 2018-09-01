<template>
  <div ref="dom" class="charts"></div>
</template>

<script>
import echarts from 'echarts'
import tdTheme from './theme.json'
echarts.registerTheme('tdTheme', tdTheme)
export default {
  name: 'ChartRadar',
  props: {
    value: Array,
    text: String,
    subtext: String,
    data: Array
  },
  mounted () {
    this.$nextTick(() => {
      // let legend = this.value.map(_ => _.name)
      let dom = echarts.init(this.$refs.dom, 'tdTheme')
      dom.clear()
      let option = {
        title: {
          text: 'Confusion Matrix'
        },
        tooltip: {
          trigger: 'axis'
        },
        // legend: {
        //   x: 'center',
        //   data: ['某软件', '降水量', '降量', '量']
        // },
        radar: [{
          indicator: [{
            text: 'real:0',
            max: 100
          },
          {
            text: 'pred:0',
            max: 100
          },
          {
            text: 'real:1',
            max: 100
          },
          {
            text: 'pred:1',
            max: 100
          }],
          center: ['50%', '60%'],
          radius: 100
        }],
        series: [{
          type: 'radar',
          tooltip: {
            trigger: 'item'
          },
          itemStyle: {
            normal: {
              areaStyle: {
                type: 'default'
              }
            }
          },
          data: this.data
        }

        ]
      }

      dom.setOption(option)
    })
  },
  watch: {
    data: function (val) {
      let dom = echarts.init(this.$refs.dom, 'tdTheme')
      dom.clear()
      let option = {
        title: {
          text: 'Confusion Matrix'
        },
        tooltip: {
          trigger: 'axis'
        },
        // legend: {
        //   x: 'center',
        //   data: ['某软件', '降水量', '降量', '量']
        // },
        radar: [{
          indicator: [{
            text: 'real:0',
            max: 100
          },
          {
            text: 'pred:0',
            max: 100
          },
          {
            text: 'real:1',
            max: 100
          },
          {
            text: 'pred:1',
            max: 100
          }],
          center: ['50%', '60%'],
          radius: 100
        }],
        series: [{
          type: 'radar',
          tooltip: {
            trigger: 'item'
          },
          itemStyle: {
            normal: {
              areaStyle: {
                type: 'default'
              }
            }
          },
          data: this.data
        }

        ]
      }

      dom.setOption(option)
    }

  }
}
</script>

<style lang="less">
.charts{
  //
}
</style>
