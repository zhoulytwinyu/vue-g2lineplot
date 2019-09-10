<template>
  <div class="app-main">
    <input v-model="url" type="text"/>
    <button v-on:click="fetchData()">Load</button>
    <div v-if="error!==null">
      {{error}}
    </div>
    <fieldset>
      <p v-if="chartData===null">Loading</p>
      <G2LinePlot v-else v-bind:data="chartData"/>
    </fieldset>
  </div>
</template>

<script>
import G2LinePlot from './components/G2LinePlot.vue';

export default {
  name: 'app',
  components: {
    G2LinePlot
  },
  data: function(){
    return {
      url:"https://api.unibit.ai/api/terminal?apiname=economic_indicators&indicatorName=San%20Francisco%20Tech%20Pulse&ecArea=u.s&startDate=9/8/2019&endDate=9/9/2019&accessKey=DEMO",
      chartData: null,
      error: null
    };
  },
  methods: {
    fetchData: function(){
      this.error = null;
      fetch(this.url)
        .then(response =>
          response.json()
        )
        .then(data => {
          this.chartData = data;
        })
        .catch(e => {
          this.error = e;
        })
    }
  },
  created: function() {
    this.fetchData();
  }
}
</script>

<style scoped>
div.app-main {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  text-align: center;
}

.app-main input {
  width: 100%;
}

button {
  width: 100%;
}

</style>
