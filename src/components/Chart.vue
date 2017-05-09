<template>
  <div class="chart">
    <canvas class="chart-canvas"></canvas>
  </div>
</template>

<script>
import Chart from 'chart.js'
import distinctColors from 'distinct-colors'

var chartOptions = {
    type: 'bar',
    data: {
        labels: [],
        datasets: [],
    },
    options: {
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero: true
                },
            }],
        },
    },
};

export default {
  name: 'chart',
  props: {
    type: {
      type: String,
      default: 'bar',
    },
    datasets: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      chart: null,
    };
  },
  mounted() {
    chartOptions.type = this.type;
    this.chart = new Chart(this.$el.querySelector('.chart-canvas'), chartOptions);
    this.updateChart();
  },
  watch: {
    datasets(){
      this.updateChart();
    },
  },
  computed: {
    numberLabels(){
      return this.chart.data.labels.length;
    }
  },
  methods: {
    updateChart() {
      var datasets = this.chart.data.datasets;
      var labels = this.chart.data.labels;
      var colors = distinctColors(this.numberLabels);

      var datasetsNew = this.datasets.sets.map((d, i) => {
        if(!this.chart.data.datasets[i])
          this.chart.data.datasets[i] = {};

        var dataset = this.chart.data.datasets[i];
        dataset.label = d.label;
        dataset.data = d.data;
        dataset.backgroundColor = colors[i];

        return dataset;
      });


      labels.splice(0, labels.length);
      labels.push.apply(labels, this.datasets.labels);
      datasets.splice(0, datasets.length);
      datasets.push.apply(datasets, datasetsNew);
      this.chart.update();
    },
    // add(labelList, valueList, index) {
    //   var labels = labelList instanceof Array ? labelList : [labelList];
    //   var values = valueList instanceof Array ? valueList : [valueList];
    //   var i = index === undefined ? 0 : index;
    //
    //   this.chart.data.labels.splice(i, 0, labels);
    //   this.chart.data.datasets[0].data.splice(i, 0, values);
    //   this.chart.update()
    // }
  }
}
</script>

<style scoped>

</style>
