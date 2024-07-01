<template>
  <div class="canvas-position" @click="gaTrack">
    <canvas
      ref="canvas"
      :width="canvasWidth * 2"
      :height="canvasHeight * 2"
      role="img"
      :style="{
        display: 'block',
        'box-sizing': 'border-box',
        height: canvasHeight + 'px',
        width: canvasWidth + 'px'
      }"
    ></canvas>
    <div
      class="tool-tip"
      :style="{ left: toolTipLeft + 'px', top: toolTipTop + 'px' }"
      v-if="currentIndex !== -1"
    >
      <div class="year">
        {{
          `${chartData.charts[currentIndex].month},${chartData.charts[currentIndex].year}`
        }}
      </div>
      <div class="avg_codes">
        <span>{{ chartData.code_num_chart }}</span>
        <span>{{ chartData.charts[currentIndex].avg_codes }}</span>
      </div>
      <div class="best_discount">
        <span>{{ chartData.best_discount_chart }}</span>
        <span>{{ chartData.charts[currentIndex].best_discount }}</span>
      </div>
    </div>
    <div class="avg-code">
      {{ 'Avg codes:' + avg }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'BarChart',
  props: {
    chartData: {
      type: [Array, Object],
      default() {
        return {}
      }
    },
    barSpacingNum: {
      type: Number,
      default() {
        return 16
      }
    }
  },
  data() {
    return {
      data: this.chartData.charts,
      barSpacing: this.barSpacingNum * 2,
      canvasWidth: 840,
      canvasHeight: 360,
      barWidth: 52 * 2,
      chartHeight: 320 * 2,
      animationDuration: 2000,
      codeNumHeight: (300 / this.chartData?.most_codes?.content) * 2,
      startTime: null,
      currentTipData: {},
      legend: [],
      currentIndex: -1,
      onceMove: -1,
      toolTipLeft: 0,
      toolTipTop: 0,
      avg: 0,
      avgArr: []
    }
  },
  mounted() {
    this.avg = (
      this.data?.reduce(
        (accumulator, item) => accumulator + item.avg_codes,
        0
      ) / this.data.length
    ).toFixed(2)
    this.startTime = performance.now()
    if (process.client && this.$refs.canvas) {
      // 根据视图更新宽高
      this.canvasWidth =
        document?.querySelector('.canvas-position')?.clientWidth || 840

      this.barWidth = Math.floor(
        ((this.canvasWidth -
          (this.chartData?.charts?.length + 1) * this.barSpacingNum) /
          this.chartData?.charts?.length) *
          2
      )
      // this.setupCanvas(this.$refs.canvas)

      // 绘制
      requestAnimationFrame(this.animateChart)

      // 监听窗口大小变化事件
      window?.addEventListener('resize', this.resizeCanvas)
    }
  },
  methods: {
    gaTrack() {
      this.$analytics(`/couponhistory/histogram`)
    },
    setupCanvas(canvas) {
      var dpr = window.devicePixelRatio || 1
      var rect = canvas.getBoundingClientRect()
      canvas.width = rect.width * dpr
      canvas.height = rect.height * dpr
      canvas.style.width = rect.width + 'px'
      canvas.style.height = rect.height + 'px'
      const ctx = canvas?.getContext('2d')
      ctx.scale(dpr, dpr)
      return canvas
    },
    resizeCanvas() {
      this.canvasWidth =
        document?.querySelector('.canvas-position')?.clientWidth || 840

      this.barWidth = Math.floor(
        ((this.canvasWidth -
          (this.chartData?.charts?.length + 1) * this.barSpacingNum) /
          this.chartData?.charts?.length) *
          2
      )
      requestAnimationFrame(this.animateChart)
    },

    animateChart(timestamp) {
      if (!this.startTime) {
        this.startTime = timestamp
      }
      const progress = Math.min(
        (timestamp - this.startTime) / this.animationDuration,
        1
      )
      this.drawChart(progress)

      if (progress < 1) {
        requestAnimationFrame(this.animateChart)
      } else {
        this.watchCanvas(this.$refs.canvas)
        this.drawAvg()
      }
    },
    drawChart(progress) {
      const canvas = this.$refs.canvas
      const ctx = canvas?.getContext('2d')
      if (!ctx) {
        return
      }

      // 清除画布
      ctx.clearRect(0, 0, this.canvasWidth * 2, this.canvasHeight * 2)

      // 绘制平均线
      ctx.setLineDash([10, 6])
      ctx.beginPath()
      ctx.moveTo(0, this.chartHeight - this.avg * this.codeNumHeight)
      ctx.lineTo(
        this.canvasWidth * 2,
        this.chartHeight - this.avg * this.codeNumHeight
      )
      ctx.strokeStyle = '#bbb'
      ctx.stroke()
      ctx.setLineDash([])
      this.avgArr = [
        0,
        (this.chartHeight - this.avg * this.codeNumHeight - 20) / 2
      ]

      // 绘制y轴实线，刻度线
      const lineNums = this.chartHeight / 50 / 2
      let currentLine = 0
      while (currentLine <= lineNums) {
        ctx.lineWidth = 0.2 * 2
        ctx.beginPath()
        // ctx.setLineDash([2, 2]) //实线和空白的比例
        ctx.moveTo(0, this.chartHeight - currentLine * 50 * 2)
        ctx.lineTo(
          this.canvasWidth * 2,
          this.chartHeight - currentLine * 50 * 2
        )
        currentLine += 1
        ctx.stroke()
      }

      // 绘制柱状图
      this.data.forEach((item, index) => {
        const x = index * (this.barWidth + this.barSpacing) + 16
        const y =
          this.chartHeight - item.avg_codes * this.codeNumHeight * progress
        ctx.fillStyle = item.is_highest_best_discount
          ? '#FF928E'
          : item.is_highest_avg_codes
          ? '#82BAFF'
          : 'rgba(0, 0, 0, 0.08)'
        // 记录每个柱状图的坐标
        this.legend[index] = {
          x: x / 2,
          y: y / 2,
          barHeight: (item.avg_codes * this.codeNumHeight) / 2
        }
        // 绘制柱状图
        // ctx.fillRect(x, y, this.barWidth, item.avg_codes * this.codeNumHeight * progress )

        if (item.avg_codes) {
          
          ctx.beginPath()
          ctx.moveTo(x - 8 * 2, y)
          ctx.lineTo(x + this.barWidth - 8 * 2, y)
          ctx.quadraticCurveTo(
            x + this.barWidth,
            y,
            x + this.barWidth,
            y + 8 * 2
          )
          ctx.lineTo(
            x + this.barWidth,
            y + item.avg_codes * this.codeNumHeight * progress
          )
          ctx.quadraticCurveTo(
            x + this.barWidth,
            y + item.avg_codes * this.codeNumHeight * progress,
            x + this.barWidth - 8 * 2,
            y + item.avg_codes * this.codeNumHeight * progress
          )
          ctx.lineTo(x, y + item.avg_codes * this.codeNumHeight * progress)
          ctx.quadraticCurveTo(
            x,
            y + item.avg_codes * this.codeNumHeight * progress,
            x,
            y + item.avg_codes * this.codeNumHeight * progress - 8 * 2
          )
          ctx.lineTo(x, y + 8 * 2)
          ctx.quadraticCurveTo(x, y, x + 8 * 2, y)
          ctx.closePath()
          ctx.fill()
        }

        // 柱状图上的数字
        ctx.fillStyle = '#000'
        ctx.font = '400 24px Poppins'
        ctx.fillText(item.avg_codes, x + this.barWidth / 2 - 5 * 2, y - 5 * 2)

        // 柱状图底部月份
        ctx.fillStyle = '#000'
        ctx.font = '400 24px Poppins'
        ctx.fillText(
          item.month,
          x + this.barWidth / 2 - 10 * 2,
          this.chartHeight + 20 * 2
        )

        let currentMonthIndex = new Date().getMonth()
        let everyChartHeight = item.avg_codes * this.codeNumHeight * progress
        if (index > currentMonthIndex ) {
          const lineNum =
            Number((this.barWidth / 15 / 2).toFixed(2)) +
            Number((everyChartHeight / 15 / 2).toFixed(2))
          for (let i = 0; i <= lineNum; i++) {
            let startX = x
            let startY = y
            let endX = 0
            let endY = 0
            // 计算开始坐标，横坐标不变的线条
            if (i < (everyChartHeight / 15 / 2).toFixed(2)) {
              startX = x
              startY = this.chartHeight - i * 15 * 2
            } else {
              // 纵坐标不变
              startX =
                x +
                (i -
                  (everyChartHeight / 15 / 2).toFixed(2)) *
                  15 * 2
              startY = y
            }
            if (i < (this.barWidth / 15 / 2).toFixed(2)) {
              endX = x + i * 15 * 2
              endY = this.chartHeight
            } else {
              endX = x + this.barWidth
              endY =
                this.chartHeight -
                (i - (this.barWidth / 15 / 2).toFixed(2)) * 15 * 2
            }
            ctx.strokeStyle = '#fff'
            ctx.lineWidth = 4
            ctx.beginPath()
            ctx.moveTo(startX, startY)
            ctx.lineTo(endX, endY)
            
            ctx.stroke()
          }
        }

        // 特殊的月份
        if (item.is_current_month) {
          const rectX = x + 9 * 2
          const rectY = this.chartHeight + 8 * 2
          const rectWidth = 34 * 2
          const rectHeight = 18 * 2
          ctx.fillStyle = '#000'
          ctx.beginPath()
          ctx.moveTo(rectX + 9, rectY)
          ctx.lineTo(rectX + rectWidth - 9, rectY)
          ctx.quadraticCurveTo(
            rectX + rectWidth,
            rectY,
            rectX + rectWidth,
            rectY + 9
          )
          ctx.lineTo(rectX + rectWidth, rectY + rectHeight - 9)
          ctx.quadraticCurveTo(
            rectX + rectWidth,
            rectY + rectHeight,
            rectX + rectWidth - 9,
            rectY + rectHeight
          )
          ctx.lineTo(rectX + 9, rectY + rectHeight)
          ctx.quadraticCurveTo(
            rectX,
            rectY + rectHeight,
            rectX,
            rectY + rectHeight - 9
          )
          ctx.lineTo(rectX, rectY + 9)
          ctx.quadraticCurveTo(rectX, rectY, rectX + 9, rectY)
          ctx.closePath()
          ctx.fill()

          ctx.fillStyle = '#fff'
          ctx.font = '500 24px Poppins'
          ctx.fillText(
            item.month,
            x + this.barWidth / 2 - 11 * 2,
            rectY + 13 * 2
          )
        }
      })
    },
    watchCanvas(canvas) {
      canvas?.addEventListener('mousemove', ev => {
        ev = ev || window.event
        this.currentIndex = -1
        for (var i = 0; i < this.legend.length; i++) {
          if (
            ev.offsetX > this.legend[i].x &&
            ev.offsetX < this.legend[i].x + this.barWidth &&
            ev.offsetY > this.legend[i].y &&
            ev.offsetY < this.legend[i].y + this.legend[i].barHeight
          ) {
            this.currentIndex = i
          }
        }
        this.toolTipLeft =
          -82 + this.legend[this.currentIndex]?.x + this.barWidth / 2 / 2
        this.toolTipTop = -77 + this.legend[this.currentIndex]?.y - 28
        this.drawHover()
      })
    },
    drawHover() {
      if (this.currentIndex !== -1) {
        if (this.onceMove === -1) {
          this.onceMove = this.currentIndex
          this.$refs.canvas.style.cursor = 'pointer'
        }
      } else {
        if (this.onceMove !== -1) {
          this.onceMove = -1
          this.$refs.canvas.style.cursor = 'inherit'
        }
      }
    },
    drawAvg() {
      let avgCode = document.querySelector('.avg-code')
      avgCode.style.left = this.avgArr[0] + 'px'
      avgCode.style.top = this.avgArr[1] + 'px'
    }
  }
}
</script>

<style  lang='scss' scoped>
.canvas-position {
  position: relative;
}
.tool-tip {
  position: absolute;
  width: 154px;
  background: #ffffff;
  box-shadow: 0px 6px 24px 0px rgba(0, 0, 0, 0.16);
  padding: 8px 12px;
  border-radius: 8px;
  color: #555;
  font-size: 12px;
  top: 0;
  left: 0;
  z-index: 1;
  .avg_codes {
    margin: 2px 0;
  }
  &::before {
    position: absolute;
    bottom: -9px;
    left: 80px;
    content: '';
    border-top: 6px solid transparent;
    border-right: 6px solid #fff;
    border-bottom: 6px solid transparent;
    border-left: none;
    transform: rotate(-90deg);
    filter: drop-shadow(-3px 0 2px rgba(0, 0, 0, 0.15));
  }

  .year {
    font-weight: 600;
    font-size: 14px;
    color: #222222;
    line-height: 22px;
    padding-bottom: 4px;
    border-bottom: 1px solid #eeeeee;
  }
  .avg_codes,
  .best_discount {
    display: flex;
    justify-content: space-between;
    span {
      font-weight: 400;
      font-size: 12px;
      color: #222222;
      line-height: 18px;
    }
  }
}
.avg-code {
  position: absolute;
  left: 0;
  top: 0;
  background: #ffffff;
  box-shadow: 0px 2px 8px 0px rgba(0, 0, 0, 0.12);
  border-radius: 6px;
  padding: 4px 8px;
  font-weight: 600;
  font-size: 12px;
  color: #222222;
  line-height: 18px;
}
</style>