<script>
import Chart from 'chart.js'
import { Line, mixins } from 'vue-chartjs'
const { reactiveProp } = mixins

export default {
  extends: Line,
  mixins: [reactiveProp],
  props: {
    chartData: {
      type: Object,
      default: () => {}
    },
    options: {
      type: Object,
      default: () => {}
    }
  },
  mounted() {
    this.addPlugin({
      beforeRender: chart => {
        if (chart.config.options.showAllTooltips) {
          chart.pluginTooltips = []
          chart.config.data.datasets.forEach((dataset, i) => {
            chart.getDatasetMeta(i).data.forEach((sector, j) => {
              chart.pluginTooltips.push(
                new Chart.Tooltip(
                  {
                    _chart: chart.chart,
                    _chartInstance: chart,
                    _data: chart.data,
                    _options: chart.options.tooltips,
                    _active: [sector]
                  },
                  chart
                )
              )
            })
          })
          chart.options.tooltips.enabled = false
        }
      },
      afterDraw: (chart, easing) => {
        if (chart.config.options.showAllTooltips) {
          if (!chart.allTooltipsOnce) {
            if (easing !== 1) {
              return
            }
            chart.allTooltipsOnce = true
          }

          chart.options.tooltips.enabled = true
          Chart.helpers.each(chart.pluginTooltips, tooltip => {
            tooltip.initialize()
            tooltip.update()
            tooltip.transition(easing).draw()
          })
          chart.options.tooltips.enabled = false
        }
      }
    })
    this.renderChart(this.chartData, this.options)
  }
}
</script>
