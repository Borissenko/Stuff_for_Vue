<template>
  <div class="main-wrapper">
    <div class="result-wrapper">
      <chart v-if="calmDownAfterResizing"
             :indicatorNames="chartData.indicatorNames"
             :knowledgeChartData="chartData.knowledgeChartData"
             :interestsChartData="chartData.interestsChartData"
             class="conclusion-chart__chart"
      />
    </div>
  </div>
</template>

<script>
  import Chart from './Chart'
  
  export default {
    components: {
      Chart
    },
    data: () => ({
      calmDownAfterResizing: true,
      chartData: {   // Против часовой стрелки, начиная от 12-00 (при "startAngle: 90").
        indicatorNames: ['Проведение исследований', 'Экономика и финансы', 'Маркетинг и продажи', 'Управление командой', 'Создание и развитие продукта'],
        knowledgeChartData: [60, 100, 51, 56, 80],
        interestsChartData: [100, 25, 30, 77, 44],
      }
    }),
    methods: {
      reportWindowSize() {
        this.calmDownAfterResizing = false
        this.$nextTick(() => this.calmDownAfterResizing = true)
      }
    },
    created() {
      window.addEventListener('resize', this.reportWindowSize);
    },
    destroyed() {
      window.removeEventListener('resize', this.reportWindowSize)
    }
  }
</script>

<style scoped lang="scss">
  .main-wrapper {
    .result-wrapper {
      width: 100%;
      display: flex;
      flex-flow: column;
      align-items: center;
    }
  }

</style>
