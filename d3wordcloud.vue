<template>
  <div class="word-cloud">
        <svg id="wordcloud" width="100%" height="74vh">
        </svg>
  </div>
</template>

<script>
import * as d3 from "d3";
import * as cloud from 'd3-cloud';
export default {
  components:{
  },
  props:{},
  data(){
    return {
        words:[
            { text: 'Vue', value: 1000 },
            { text: 'js', value: 200 },
            { text: 'is', value: 800 },
            { text: 'very cool', value: 190 },
            { text: 'lunch', value: 100 },
        ],
        fontSizeMapper: word => Math.log2(word.value) * 5,
    }
  },
  computed:{},
  mounted(){
      this.init();
  },
  methods:{
      init() {
          let layout = cloud()
              .size([500, 500])
              .words([
                  "Hello", "world", "normally", "you", "want", "more", "words",
                  "than", "this"].map(function (d) {
                      return { text: d, size: 10 + Math.random() * 90};
                  }))
              .padding(5)
              .rotate(function () { return ~~(Math.random() * 2) * 30; })
              .font("Impact")
              .fontSize(function (d) { return d.size; })
              .on("end", draw);

          layout.start();
          setInterval(() => {
              layout.start()
          }, 4000);
          function draw(words) {
              let svg = d3.select("#wordcloud");
              svg.selectAll("*").remove();
              svg.attr("width", layout.size()[0])
                  .attr("height", layout.size()[1])
              let g = svg.append("g")
                  .attr("transform", "translate(" + layout.size()[0] / 2 + "," + layout.size()[1] / 2 + ")")
              let text = g.selectAll("text")
                  .data(words)
                  .enter().append("text")
                  .attr("transform", function (d) {
                      return "translate(" + [Math.random() * 1000 - 500, Math.random() * 1000 - 500] + ")";
                  })
                  .transition().duration(700).delay(function (d, i) { return i * 30; })
                  .style("font-size", function (d) { return d.size + "px"; })
                  .style("font-family", "Impact")
                  .attr("text-anchor", "middle")
                  .text(function (d) { return d.text; })
                  .attr("transform", function (d) {
                      return "translate(" + [d.x, d.y] + ")rotate(" + d.rotate + ")" + "scale(1.2)";
                  })
                  .transition().duration(150)
                  .attr("transform", function (d) {
                      return "translate(" + [d.x, d.y] + ")rotate(" + d.rotate + ")" + "scale(0.8)";
                  })
                  .transition().duration(150)
                  .attr("transform", function (d) {
                      return "translate(" + [d.x, d.y] + ")rotate(" + d.rotate + ")" + "scale(1.1)";
                  })
                  .transition().duration(150)
                  .attr("transform", function (d) {
                      return "translate(" + [d.x, d.y] + ")rotate(" + d.rotate + ")" + "scale(1)";
                  })
                  .transition().duration(700).delay(2000)
                  .attr("transform", function (d) {
                      return "translate(" + [Math.random() * 2000 - 1000, Math.random() * 2000 - 1000] + ")";
                  })
                  .style('opacity',0)
          }
      }
  },
}
</script>
<style scoped>
</style>