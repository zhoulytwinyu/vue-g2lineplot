<template>
  <div class="chart-container">
    <h1 class='a'>{{selection}}</h1>
    <div ref="chart" class='1'>
    </div>
    <select v-bind:value="selection"
            v-on:input="userSelection=$event.target.value">
      <option v-for="option in options"
              v-bind:value="option"
              v-bind:key="option">
         {{option}}
      </option>
    </select>
  </div>
</template>

<script>
import G2 from '@antv/g2';

const G2LinePlot = {
  name: 'G2LinePlot',
  $chart: null,
  props: {
    data: Object
  },
  data: function() {
    return {
      userSelection:null
    };
  },
  computed: {
    options: function(){
      let {data} = this;
      if (data===null) {
        return [];
      }
      return Object.keys(data).sort();
    },
    selection: function(){
      let {options,userSelection} = this;
      if (options.length===0) {
        return null;
      }
      else if (userSelection===null) {
        return options[0];
      }
      else {
        return userSelection;
      }
    }
  },
  mounted: function(){
    let {data,selection} = this;
    let chart = this.$chart = new G2.Chart({
      container: this.$refs.chart,
      width: 800,
      height: 400
    });
    chart.source(
      data[selection],
      { date: {
          alias: 'Date',
          type: 'time',
          mask: 'YYYY-mm-dd',
          tickCount:5,
          nice: false
          },
        value: {
          alias:'Value',
          min: 0
        }
      }
    );
    chart.line().position('date*value').size(2);
    chart.render();
  },
  updated: function(){
    let {data,selection} = this;
    let chart = this.$chart;
    chart.changeData(data[selection]);
  }
};

export default G2LinePlot;
</script>

<style scoped>
div.chart-container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  text-align: center;
}

.chart-container h1 {
  font-size: 14px;
}
</style>
