<template>
    <div v-if="stockDetails">  
      <div class="row justify-content-center">
        <div class="col-1"></div>
        <div class="col-9"></div>
        <div class="col-1"></div>  
      </div>
      <div class="chart-container">
        <canvas ref="canvas"></canvas>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import { Chart, registerables } from 'chart.js';
  import { nextTick } from 'vue';
  Chart.register(...registerables);
  
  export default {
    name: 'StockDetails',
    props: {
      ticker: {
        type: String,
        required: true
      },
      companyName: {
        type: String,
        required: true
      }
    },
    data() {
      return {
        stockDetails: null,
        chart: null // Chart.js 인스턴스를 저장할 변수
      };
    },
    watch: {
      ticker: 'fetchStockDetails'
    },
    mounted() {
      this.fetchStockDetails();
    },
    methods: {
      async fetchStockDetails() {
        try {
          const response = await axios.get(`http://localhost:8080/stocks/${this.ticker}`);
          console.log('Response data:', response.data); // 응답 데이터 구조 확인
          this.stockDetails = response.data;
          await nextTick(); // DOM 업데이트 후 차트를 렌더링
          this.renderChart(); // 데이터를 가져온 후 차트를 렌더링합니다.
        } catch (error) {
          console.error('Error fetching stock details:', error);
        }
      },    
      renderChart() {
        if (this.chart) {
          this.chart.destroy(); // 기존 차트를 파괴하여 메모리 누수를 방지합니다.
        }
  
        // 유효성 검사 추가
        if (!this.stockDetails) {
          console.error('Stock data is missing or invalid');
          return;
        }
  
        const labels = this.stockDetails.map(data => data.date);
        const data = this.stockDetails.map(data => data.close);

        const ctx = this.$refs.canvas.getContext('2d');
        if (!ctx) {
          console.error('Canvas context not found');
          return;
        }
  
        this.chart = new Chart(ctx, {
          type: 'line',
          data: {
            labels: labels,
            datasets: [{
              label: `${this.companyName} Closing Price`,
              backgroundColor: 'rgba(75, 192, 192, 0.2)',
              borderColor: 'rgba(75, 192, 192, 1)',
              borderWidth: 1,
              data: data
            }]
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
              x: {
                ticks: {
                  maxRotation: 45,
                  minRotation: 45,
                  callback: function(value) {
                    const date = new Date(labels[value]);
                    const options = { year: 'numeric', month: '2-digit', day: '2-digit' };
                    return date.toLocaleDateString('en-US', options);
                  }
                }
              },
              y: {
                beginAtZero: false
              }
            },
            plugins: {
              tooltip: {
                callbacks: {
                  label: function(context) {
                    let label = context.dataset.label || '';
                    if (label) {
                      label += ': ';
                    }
                    if (context.parsed.y !== null) {
                      label += context.parsed.y.toFixed(2); // 종가 값 표시
                    }
                    return label;
                  }
                }
              }
            }
          }
        });
      }
    }
  };
  </script>
  
  <style scoped>
  .chart-container {
    position: relative;
    display: flex;
    justify-content: center;
    width: 80%;
    height: 400px;
    margin: 0 auto;
  }
  </style>
  