<template>
  <div id="app">
    <main>
      <button id="random-data-btn" class="btn btn-primary" @click="randomData">
        Generate random data
      </button>
      <div>
        <label for="greater-than">Show only sources with prospects greater than: {{ greaterThan }}</label>
        <input id="greater-than" type="range" min="0" max="99" v-model.lazy="greaterThan">
      </div>
      <div class="row">
        <div class="sources-list-container col-sm-5 col-sm-push-7">
          <div class="list-group">
            <div class="list-group-item" v-for="src in sources">
              <h4 class="list-group-item-heading">{{src.source}}</h4>
              <p class="list-group-item-text">
                Prospects: {{src.prospects}}<br>
                Prospects with event list: {{src.prospects_with_event}}
              </p>
              <button class="remove-button btn btn-default" title="Remover" @click="removeSource(src)">
                <span class="glyphicon glyphicon-trash"></span>
              </button>
            </div>
          </div>
        </div>
        <div class="charts-container col-sm-7 col-sm-pull-5">
          <chart :datasets="chartData"></chart>
          <chart :type="'doughnut'" :datasets="chartData"></chart>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import Chart from './components/Chart'

export default {
  name: 'app',
  components: {
    Chart
  },
  data(){
    return {
      sources_: [],
      greaterThan: 0,
    }
  },
  created(){
    this.randomData();
  },
  methods: {
    randomData() {
      const range = 100;
      var sources = ['Website', 'Facebook', 'Instagram', 'Twitter', 'TV', 'Github', 'Email'];
      var data = sources.map((source) => {
        return {
          source,
          prospects: Math.round(Math.random() * range),
          prospects_with_event: Math.round(Math.random() * range),
        };
      });

      this.sources_ = data;
    },
    removeSource(source){
      var index;
      var hasSource = this.sources_.some((s, i) => {
        index = i;
        return s.source == source.source;
      });

      if(hasSource)
        this.sources_.splice(index, 1);
    }
  },
  computed: {
    sources(){
      return this.sources_.filter((s) => {
        return s.prospects > this.greaterThan;
      });
    },
    chartData(){
      let labels = this.sources.map((s) => {
        return s.source;
      });

      let prospectsList = this.sources.map((s) => {
        return s.prospects;
      });

      let prospectsWithEventList = this.sources.map((s) => {
        return s.prospects_with_event;
      });

      let sets = [
        {
          label: 'Prospects',
          data: prospectsList,
        },
        {
          label: 'Prospects with event',
          data: prospectsWithEventList,
        },
      ];

      return {
        labels: labels,
        sets: sets,
      };
    },
  }
}
</script>

<style lang='scss'>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 10px 10px 0px 10px;

  #random-data-btn{
    margin-bottom: 1em;
  }

  #greater-than{
    width: 90%;
    display: inline-block;
  }

  .sources-list-container{
    height: 100%;
    overflow-y: hidden;

    .list-group-item{
      text-align: left;
      width: 100%;

      .remove-button{
        position: absolute;
        top: 30%;
        right: 10px;
        margin-right: auto;
        margin-left: auto;
        padding: 5px;
        display: block;
      }
    }
  }
}
</style>
