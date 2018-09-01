<template>
    <div>
         <Row :gutter="20" style="margin-top: 8px;">
      <i-col span="14">
        <Card shadow >
          <Row :gutter="20">
            <i-col span="12">
            <chart-radar style="height: 400px;" :data=radarData></chart-radar>
            </i-col>
            <i-col span="12">
              <div id='hotMap' style="height: 400px; width:75%"> </div>
            </i-col>
          </Row>
        </Card>
      </i-col>
      <i-col span="10">
        <div shadow style="height:400px">
            <Row :gutter="20" >
                <card shadow style="height:120px">
                   <p slot="title" class="card-title">
                        <Icon type="android-map"></Icon>
                        Decision Criterion
                    </p>
                    <div class="data-source-row">
                        <Slider v-model="sliderValue" show-input @on-change=sliderChange></Slider>
                    </div>

                </card>
            </Row>
            <Row :gutter="20" style="margin-top:10px">
                <card shadow style="height:302px">
                  <p slot="title" class="card-title">
                        <Icon type="android-map"></Icon>
                        Evaluation Metrics
                    </p>
                    <div class="data-source-row">
                        <Table :height="height" :columns="columns" :data="tableData"></Table>

                    </div>
                </card>
            </Row>
        </div>
      </i-col>
    </Row>

      <card shadow style="margin-top:10px;margin-left: -10px;margin-right: -10px;">
        <Row :gutter="20"  >
        <i-col span="12">
          <div id="local-line-1" style="height:300px;width:80%"></div>
        </i-col>
        <i-col span="12">
          <div id="local-line-2" style="height:300px;width:80%"></div>
        </i-col>
         </Row>
      </card>

    </div>
</template>

<script>
import echarts from 'echarts'
import { ChartRadar } from '_c/charts'
import tdTheme from '_c/charts/theme.json'
echarts.registerTheme('tdTheme', tdTheme)
export default {
  name: 'dataSourcePie',
  components: {
    ChartRadar
  },
  data () {
    return {
      //
      sliderValue: 0,
      height: 230,
      columns: [
        {
          title: 'metric',
          key: 'metric'
        },
        {
          title: 'score',
          key: 'score',
          sortable: true
        }
      ],
      radarData: [{
        value: [60, 60, 0, 0],
        name: '某软件',
        type: 'circle'

      },

      {
        value: [0, 73, 73, 0],
        name: '降水量'

      },
      {
        value: [0, 0, 79, 79],
        name: '降量'

      },
      {
        value: [45, 0, 0, 45],
        name: '量'

      }],
      tableData: [
        {
          metric: 'sensitivity',
          score: '98%'
        },
        {
          metric: 'specificity',
          score: '96.1%'
        },
        {
          metric: 'precision',
          score: '96.0%'
        },
        {
          metric: 'recall',
          score: '98%'
        },
        {
          metric: 'balanced_accuracy',
          score: '97%'
        }

      ],
      hotChartData: [[0, 0, 0], [0, 1, 0], [0, 1, 0], [0, 2, 9], [1, 1, 10], [1, 2, 6], [2, 0, 13], [2, 1, 0], [2, 2, 0]],
      lineChart1Data: [820, 932, 901, 934, 1290, 1330, 1320],
      lineChart2Data: [820, 932, 901, 934, 1290, 1330, 1320]
    }
  },
  methods: {
    hotChart () {
      var dataSourcePie = echarts.init(document.getElementById('hotMap'), 'tdTheme')
      dataSourcePie.clear()
      app.title = '坐标系上的热力图'

      var hours = ['setosa', 'versicolor', 'virginica']
      var days = ['setosa', 'versicolor', 'virginica']

      var data = this.hotChartData
      data = data.map(function (item) {
        return [item[1], item[0], item[2] || '-']
      })

      const option = {
        tooltip: {
          position: 'top'
        },
        animation: false,
        grid: {
          // height: '50%',
          left: '20%'
        },
        xAxis: {
          type: 'category',
          data: hours,
          splitArea: {
            show: true
          }
        },
        yAxis: {
          type: 'category',
          data: days,
          splitArea: {
            show: true
          }
        },
        visualMap: {
          min: 0,
          max: 15,
          calculable: true,
          // orient: 'horizontal',
          // left: '',
          right: '-3%',
          bottom: '18%'
        },
        series: [{
          name: 'Punch Card',
          type: 'heatmap',
          data: data,
          label: {
            normal: {
              show: true

            },
            backgroundColor: 'blue'
          },
          itemStyle: {
            emphasis: {
              shadowBlur: 10,
              shadowColor: 'rgba(0, 0, 0, 0.5)'
            }
          }
        }]
      }
      dataSourcePie.setOption(option)
      window.addEventListener('resize', function () {
        dataSourcePie.resize()
      })
    },
    lineChart1 () {
      var dataSourcePie = echarts.init(document.getElementById('local-line-1'), 'tdTheme')
      dataSourcePie.clear()
      const option = {
        title: {
          text: 'Area Under the ROC curve'
        },
        xAxis: {
          type: 'category',
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
          name: 'False Positive rate'
        },
        yAxis: {
          type: 'value',
          name: 'True Positive rate'
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
        series: [{
          data: this.lineChart1Data,
          type: 'line'
        }]
      }

      dataSourcePie.setOption(option)
      window.addEventListener('resize', function () {
        dataSourcePie.resize()
      })
    },
    lineChart2 () {
      var dataSourcePie = echarts.init(document.getElementById('local-line-2'), 'tdTheme')
      dataSourcePie.clear()
      const option = {
        title: {
          text: 'Area Under the Precision-Recall curve'
        },
        xAxis: {
          type: 'category',
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
          name: 'Recall'
        },
        yAxis: {
          type: 'value',
          name: 'precision'
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
        series: [{
          data: this.lineChart2Data,
          type: 'line'
        }]
      }

      dataSourcePie.setOption(option)
      window.addEventListener('resize', function () {
        dataSourcePie.resize()
      })
    },
    sliderChange () {
      // line1
      this.lineChart1Data = []
      for (let i = 0; i < 10; i++) {
        let value = this.sliderValue + Math.random() * 21 - 10
        this.lineChart1Data.push(Math.round(value))
      }
      this.lineChart1()
      // line2
      this.lineChart2Data = []
      for (let i = 0; i < 10; i++) {
        let value = this.sliderValue + Math.random() * 21 - 10
        this.lineChart2Data.push(Math.round(value))
      }
      this.lineChart2()
      // 热力图
      this.hotChartData = []
      for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 3; j++) {
          let value = this.sliderValue + Math.random() * 21 - 10
          let data = [i, j, Math.round(value)]
          this.hotChartData.push(data)
        }
      }
      this.hotChart()
      // 雷达图
      this.radarData = []
      for (let i = 0; i < 4; i++) {
        let value = this.sliderValue + Math.random() * 40 - 10
        if (i === 0) {
          let data = {
            value: [Math.round(value), Math.round(value), 0, 0],
            name: '水量'
          }
          this.radarData.push(data)
        } else if (i === 1) {
          let data = {
            value: [0, Math.round(value), Math.round(value), 0],
            name: '降水量'
          }
          this.radarData.push(data)
        } else if (i === 2) {
          let data = {
            value: [0, 0, Math.round(value), Math.round(value)],
            name: '降'
          }
          this.radarData.push(data)
        } else {
          let data = {
            value: [Math.round(value), 0, 0, Math.round(value)],
            name: '量'
          }
          this.radarData.push(data)
        }
      }
      // 表格数据
      // tableData: [
      //   {
      //     metric: 'sensitivity',
      //     score: '98%'
      //   },
      //   {
      //     metric: 'specificity',
      //     score: '96.1%'
      //   },
      //   {
      //     metric: 'precision',
      //     score: '96.0%'
      //   },
      //   {
      //     metric: 'recall',
      //     score: '98%'
      //   },
      //   {
      //     metric: 'balanced_accuracy',
      //     score: '97%'
      //   }

      // ],
      this.tableData = []
      let metric = ['sensitivity', 'specificity', 'precision', 'recall', 'balanced_accurancy']
      for (let i = 0; i < 5; i++) {
        let value = this.sliderValue + Math.random() * 40 - 10
        let data = {
          metric: metric[i],
          score: Math.round(value) + '%'
        }
        this.tableData.push(data)
      }
    }
  },
  mounted () {
    this.hotChart()
    this.lineChart1()
    this.lineChart2()
  }
}
</script>
<style>
.ivu-slider-button{
    width: 16px;
    height: 16px;
}
.ivu-slider-wrap{
  height: 8px;
}
.ivu-slider-bar{
  height: 8px;
}
.card-title{
    display: inline-block;
    width: 100%;
    height: 12px;
    line-height: 12px;
    font-size: 14px;
    color: #17233d;
    font-weight: bold;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}
</style>
