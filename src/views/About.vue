<template>
  <div>
    echarts图表
    <div ref="echart" id="about" />
  </div>
</template>
<script>
import echarts from 'echarts'
export default {
  name: 'About',

  data () {
    return {
      options: []
    }
  },

  mounted () {
    this.myEcharts()
  },

  methods: {
    myEcharts () {
      // 基于准备好的dom，初始化echarts实例
      const myChart = echarts.init(this.$refs.echart)

      // 指定图表的配置项和数据
      const option = {
        title: {
          text: 'ECharts的使用',
          textStyle: {
            fontSize: 26,
            color: '#24d'
          },
          subtext: 'demo'
        },
        // color: ['#2f4554', '#61a0a8'],
        toolbox: {
          feature: {
            dataView: { show: true, readOnly: false },
            restore: {},
            saveAsImage: {}
          }
        },
        legend: {
          data: ['2020年月度代码贡献量'],
          textStyle: {
            // 图例文字的样式
            color: '#ccc',
            fontSize: 16
          }
        },
        xAxis: {
          data: [
            '1月',
            '2月',
            '3月',
            '4月',
            '5月',
            '6月',
            '7月',
            '8月',
            '9月',
            '10月',
            '11月',
            '12月'
          ]
        },
        yAxis: {
          // type: 'value'
        },
        series: [
          {
            name: '2020年月度代码贡献量',
            type: 'bar',
            data: [
              50000,
              20000,
              36000,
              100000,
              108808,
              208886,
              300000,
              20090,
              47999,
              380933,
              792792,
              373333
            ],
            itemStyle: {
              // 这里是重点
              color: function (params) {
                // 注意，如果颜色太少的话，后面颜色不会自动循环，最好多定义几个颜色
                var colorList = [
                  '#c23531',
                  '#2f4554',
                  '#61a0a8',
                  '#d48265',
                  '#91c7ae',
                  '#749f83',
                  '#ca8622'
                ]
                console.log(params.dataIndex)
                // 给大于颜色数量的柱体添加循环颜色的判断
                if (params.dataIndex >= colorList.length) {
                  const index = params.dataIndex - colorList.length
                  return colorList[index]
                }
                return colorList[params.dataIndex]
              }
            }
          }
        ]
      }

      // 使用刚指定的配置项和数据显示图表。
      myChart.setOption(option)
    }
  }
}
</script>
<style lang="scss">
#about {
  width: 100%;
  height: 500px;
}
</style>
