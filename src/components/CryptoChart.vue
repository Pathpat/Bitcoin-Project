<template>
    <Bar id="my-chart-id" :options="chartOptions" :data="chartData" />
</template>
  
<script>
import { Bar } from 'vue-chartjs';
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js';

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

export default {
    name: 'BarChart',
    components: { Bar },
    props: {
        euroData: {
            type: Array,
            default: () => [],
        },
        usdData: {
            type: Array,
            default: () => [],
        },
    },

    computed: {
        chartData() {
            return {
                labels: this.getLabelArray(),
                datasets: [
                    {
                        label: 'EUR Data',
                        data: this.getChartData(this.euroData),
                        backgroundColor: 'blue',
                    },
                    {
                        label: 'USD Data',
                        data: this.getChartData(this.usdData),
                        backgroundColor: 'green',
                    },
                ],
            };
        },
    },

    data() {
        return {
            chartOptions: {
                responsive: true,
            },
        };
    },

    methods: {
        getLabelArray() {
            // Generate an array of labels based on the data length
            const euroDataLength = this.euroData.length;
            const usdDataLength = this.usdData.length;
            const maxLength = Math.max(euroDataLength, usdDataLength);

            return Array.from({ length: maxLength }, (_, i) => `Data ${i + 1}`);
        },
        getChartData(dataList) {
            // Extract the formatted price data and fill with zeros if the data is not available
            return dataList.map(data => (data ? parseFloat(data.formattedPrice.replace(/[^\d.-]/g, '')) : 0));
        },
    },

    mounted() {
        this.renderChart(this.chartData, this.chartOptions);
    },
};
</script>
  