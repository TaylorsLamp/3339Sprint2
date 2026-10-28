<template>
  <div class="w-1/2">
    <canvas class="p-10" ref="zipChart"></canvas>
  </div>
</template>

<script>
import { Chart, registerables } from 'chart.js'
Chart.register(...registerables)
//define props (label and chartData)
import { ref, onMounted, onBeforeUnmount } from 'vue'
export default {
  props: {
    label: {
      type: Array
    },
    chartData: {
      type: Array
    }
    },
  setup(props) {
    // Sprint 2: reactive state replaces Options API data().
    const chartCanvas = ref(null)
    let chartInstance = null
    onMounted(async () => { //uses the data to load the chart's values
    chartInstance = new Chart(chartCanvas.value, {
      type: 'doughnut',
      data: {
        labels: props.label,
        datasets: [
          {
            borderWidth: 1,
            data: props.chartData // Update this line
          }
        ]
      },
      options: {
        plugins: {
          legend: {
            display: false
          },
          title: {
            display: true,
            text: 'Clients by Zip Code'
          }
        },
        responsive: true,
        maintainAspectRatio: true
      }
    })
  
    })
    onBeforeUnmount(() => chartInstance?.destroy())
    return { zipChart: chartCanvas }
  }
}
</script>
