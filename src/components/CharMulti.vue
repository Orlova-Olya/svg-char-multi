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
      // Массив сегментов в формате: { count: 10, color: '#013A76' }
      type: Array,
      required: true
    },
    circleCount: {
      // Значение (100% круга), процент от которого будут рассчитываться доли count сегментов от круга
      // Если не будет указан, или будет некорректный, то будет использоваться сумма count всех сегментов
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

      // circleValue - значение (100% круга), процент от которого будут рассчитываться доли count сегментов от круга
      let circleValue = this.circleCount
      // countsSum - сумма count всех сегментов из array
      const countsSum = this.array.reduce((sum, item) => sum + +item.count, 0)
      if (this.circleCount === undefined || countsSum > this.circleCount) {
        // Если circleCount не указан или
        // если countsSum больше circleCount, то для того,
        // чтобы все сегменты уместились и их размеры были относительно друг друга корректными,
        // то в качестве circleValue будет countsSum
        circleValue = countsSum
      }

      // Расчёт значений стилей svg-элементов сегментов
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
