<template>
  <svg class="dashboard-chart" :width="sideLength" :height="sideLength">
    <circle
      :cx="sideLength / 2"
      :cy="sideLength / 2"
      :r="circlesRadius"
      :stroke-width="strokeWidth"
      fill="none"
      stroke="#dbdbdb"
    />
    <circle
      v-for="(circle, index) of circles"
      :key="index"
      :cx="sideLength / 2"
      :cy="sideLength / 2"
      :r="circlesRadius"
      :stroke-width="strokeWidth"
      :stroke-dasharray="circle.strokeDasharray"
      fill="none"
      :stroke="circle.color"
      :style="{ transform: `rotate(${circle.rotateDeg}deg)` }"
      class="dashboard-chart__circle"
    />
  </svg>
</template>

<script>
export default {
  props: {
    array: {
      type: Array,
      required: true
    },
    sum: {
      type: Number,
      default: undefined
    },
    sideLength: {
      type: Number,
      default: 200
    },
    strokeWidth: {
      type: Number,
      default: 40
    }
  },

  computed: {
    circlesRadius() {
      return (this.sideLength - this.strokeWidth) / 2
    },
    circumference() {
      return 2 * Math.PI * this.circlesRadius
    },
    circles() {
      const circumference = 2 * Math.PI * this.circlesRadius
      let rotateDeg = -90

      let circleValue = this.sum
      const countsSum = this.array.reduce((sum, item) => sum + +item.count, 0)
      if (this.sum === undefined || countsSum > this.sum) {
        circleValue = countsSum
      }

      let array = []
      for (let i = 0; i < this.array.length; i++) {
        const itemPercent = +this.array[i].count / circleValue
        const strokeDasharray = itemPercent * circumference + ' ' + (1 - itemPercent) * circumference
        array.push({ strokeDasharray, rotateDeg, color: this.array[i].color })

        rotateDeg += 360 * itemPercent
      }

      return array
    }
  }
}
</script>

<style lang="scss">
.dashboard-chart {
  display: block;

  &__circle {
    transition-duration: 1s;
    transform-origin: center;
  }
}
</style>
