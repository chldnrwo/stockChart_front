<template>
    <div class="chart-container">
      <canvas ref="canvas"></canvas>
    </div>
  </template>
  
  <script>
  import { Chart, registerables } from 'chart.js';
  import axios from 'axios';
  Chart.register(...registerables);
  
  export default {
    name: 'BarChart',
    data() {
      return {
        stockData: {
          TSLA: [],
          NVDA: [],
          GOOGL: [],
          APPL: [],
          MSFT: [],
          AMZN: [],
          META: []
        }
      };
    },
    mounted() {
      this.fetchStockData();
    },
    methods: {
      fetchStockData() {
        axios.get('http://localhost:8080/stocks/M7')
          .then(response => {
            console.log('API response:', response.data);
            this.stockData = response.data;
            this.renderChart();
          })
          .catch(error => {
            console.error("Error fetching stock data: ", error);
          });
      },
      renderChart() {
        const ctx = this.$refs.canvas.getContext('2d');
        
        // 유효성 검사 추가
        if (!this.stockData.TSLA || !this.stockData.NVDA || !this.stockData.GOOGL || 
            !this.stockData.APPL || !this.stockData.MSFT || !this.stockData.AMZN || !this.stockData.META) {
          console.error('Stock data is missing');
          return;
        }
  
        const labels = this.stockData.TSLA.map(data => data.date);
        
        const datasets = [
          {
            label: 'Tesla Stock Price',
            backgroundColor: 'rgba(75, 192, 192, 0.2)',
            borderColor: 'rgba(75, 192, 192, 1)',
            borderWidth: 1,
            data: this.stockData.TSLA.map(data => data.close)
          },
          {
            label: 'Nvidia Stock Price',
            backgroundColor: 'rgba(192, 75, 75, 0.2)',
            borderColor: 'rgba(192, 75, 75, 1)',
            borderWidth: 1,
            data: this.stockData.NVDA.map(data => data.close)
          },
          {
            label: 'Google Stock Price',
            backgroundColor: 'rgba(75, 192, 75, 0.2)',
            borderColor: 'rgba(75, 192, 75, 1)',
            borderWidth: 1,
            data: this.stockData.GOOGL.map(data => data.close)
          },
          {
            label: 'Apple Stock Price',
            backgroundColor: 'rgba(192, 192, 75, 0.2)',
            borderColor: 'rgba(192, 192, 75, 1)',
            borderWidth: 1,
            data: this.stockData.APPL.map(data => data.close)
          },
          {
            label: 'Microsoft Stock Price',
            backgroundColor: 'rgba(75, 75, 192, 0.2)',
            borderColor: 'rgba(75, 75, 192, 1)',
            borderWidth: 1,
            data: this.stockData.MSFT.map(data => data.close)
          },
          {
            label: 'Amazon Stock Price',
            backgroundColor: 'rgba(192, 75, 192, 0.2)',
            borderColor: 'rgba(192, 75, 192, 1)',
            borderWidth: 1,
            data: this.stockData.AMZN.map(data => data.close)
          },
          {
            label: 'Meta Stock Price',
            backgroundColor: 'rgba(75, 192, 192, 0.2)',
            borderColor: 'rgba(75, 192, 192, 1)',
            borderWidth: 1,
            data: this.stockData.META.map(data => data.close)
          }
        ];
  
        if (ctx) {
          new Chart(ctx, {
            type: 'line',
            data: {
              labels: labels,
              datasets: datasets
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
                      //if (date.getDate() === 1) { // 매월 1일만 표시
                        const options = { year: 'numeric', month: '2-digit' };
                      return date.toLocaleDateString('en-US', options).replace('/', '/');
                      //}
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
                        label += new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(context.parsed.y);
                      }
                      return label;
                    }
                  }
                }
              }
            }
          });
        } else {
          console.error("Canvas context not found.");
        }
      }
    }
  }
  </script>
  
  <style scoped>
  .chart-container {
    position: relative;
    width: 80%;
    height: 400px;
  }
  </style>
  