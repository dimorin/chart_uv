<template>
  <div class="d3wrap">
      <h3>d3.test</h3>
  </div>
</template>

<script>
import d3 from 'd3'
export default {
    mounted(){
        var data = [50,20,30];
        d3.select('.d3wrap').selectAll("p").data(data).enter().append('p').style("width", function(d){return d+'px'});
    }
}
</script>

<style>
p{background:red;height:10px;}
</style>