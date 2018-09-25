<template>
  <div id="chart">

  </div>
</template>

<script>
import * as d3 from 'd3';
import Data from '../assets/parsed_pokemon.json';
window.test = d3;
export default {
  name: 'pokemon',
  data() {
    return {
      data: [],
      svg: null,
      circles: null,
      simulation: null,
      text: null,
      width: 900,
      height: 600,
    };
  },
  created() {
    
    Data.map((val) => {
      let copy = val;
      let name = copy.id.substr(8);
      copy['name'] = name.charAt(0).toUpperCase() + name.substr(1);
    });

    this.data = Data;

    this.simulation = d3.forceSimulation(this.data)
      .force('x', d3.forceX(this.width/2).strength(0.05))
      .force('y', d3.forceY(this.height/2).strength(0.05))
      .force('collide', d3.forceCollide((d) => {
        return (this.radius())(d.value);
      }))
      .on('tick', this.tick);
  },
  mounted() {
    this.draw();
  },
  methods: {
    //d3 seems to make some assumption about data
    //examples seem to have an id field
    draw() {
      this.svg = d3.select('#chart')
        .append('svg')
        .attr('height', this.height)
        .attr('width', this.width);

      this.circles = this.svg.selectAll('g')
        .data(this.data)
        .enter().append('g')
        .append('circle')
        .attr('class', 'artist')
        .attr('r', (d) => {
          return (this.radius())(d.value);
        })
        .attr('fill', (d) => {
          return d.color;
        });
      
      this.text = this.svg.selectAll('g')
        .data(this.data)
        .append('text')
        .attr('text-anchor', 'middle')
        .text(function (d) {
          return `${d.name} (${d.value})`;
        });
    },
    tick() {
      this.circles
      .attr('cx', function (d) {
        return d.x
      })
      .attr('cy', function (d) {
        return d.y
      })

      this.text
      .attr('x', (d) => {
        return d.x;
      })
      .attr('y', (d) => {
        return d.y
      })
    },
    radius() {
      return d3.scaleSqrt().domain([43,234]).range([10,80])
    }
  }
}
</script>

<style scoped>
</style>
