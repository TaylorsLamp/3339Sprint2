<template>
  <div>
    <canvas class="p-10" ref="attendanceChart"></canvas>
  </div>
</template>

<script>
import { Chart, registerables } from 'chart.js'
import { Colors } from 'chart.js';

Chart.register(...registerables)
Chart.register(Colors);

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
    onMounted(async () => {
    chartInstance = new Chart(chartCanvas.value, {
      type: 'bar',
      data: {
        labels: props.label,
        datasets: [
          {
            borderWidth: 1,
            data: props.chartData
          }
        ]
      },
      options: {
        scales: {
          y: {
            ticks: {
              stepSize: 1
            }
          },
          x: {
            gridLines: {
              display: false
            }
          }
        },
        plugins: {
          legend: {
            display: false
          },
          title: {
            display: true,
            text: 'Attendance Chart'
          }
        },
        responsive: true,
        maintainAspectRatio: true
      }
    })
  
    })
    onBeforeUnmount(() => chartInstance?.destroy())
    return { attendanceChart: chartCanvas }
  }
}
</script>

