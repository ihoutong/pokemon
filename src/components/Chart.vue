<template>
  <div id="chart">
  </div>
</template>

<script>
import {select, selectAll} from 'd3-selection';
import {forceSimulation, forceX, forceY, forceCollide} from 'd3-force';
import {scaleSqrt} from 'd3-scale';

import Colors from '../assets/typeColors.json';
import Original from '../assets/orig_pokemon.json';

export default {
  name: 'chart',
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
    this.parseData();

    this.simulation = forceSimulation(this.data)
      .force('x', forceX(this.width/2).strength(0.05))
      .force('y', forceY(this.height/2).strength(0.05))
      .force('collide', forceCollide((d) => {
        return this.radius(d.value) + 3;
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
      this.svg = select('#chart')
        .append('svg')
        .attr('height', this.height)
        .attr('width', this.width);

        
      this.circles = this.svg.selectAll('g')
        .data(this.data)
        .enter().append('g')
        .append('circle')
        .attr('class', 'artist')
        .attr('r', (d) => {
          return this.radius(d.value);
        })
        .attr('fill', (d) => {
          return d.color;
        })
        .on('click', this.clicked);
      
      /*
      this.text = this.svg.selectAll('g')
        .data(this.data)
        .append('text')
        .attr('text-anchor', 'middle')
        .text(function (d) {
          return `${d.type}`;
        });*/
    },
    tick() {
      this.circles.attr('cx', function (d) {
        return d.x
      }).attr('cy', function (d) {
        return d.y
      })
      /*
      this.text.attr('x', (d) => {
        return d.x;
      }).attr('y', (d) => {
        return d.y
      });*/
    },
    radius(value) {
      return scaleSqrt().domain([43,234]).range([10,80])(value)
    },
    parseData() {
      Original.map((val) => {
        const pokemonName = val.name;
        Object.keys(val).forEach((key) => {
          if (key.indexOf('against_') === -1 || val[key] <= 1){
            return;
          }

          const type = key.substr(8);
          
          if (typeof this.data[key] === 'undefined'){
            this.data[key] = {
              id: key,
              type: type.charAt(0).toUpperCase() + type.substr(1),
              value: 0,
              pokemons: [],
              color: Colors[key]
            }
          }
          
          this.data[key].value++;
          this.data[key].pokemons.push(pokemonName);
        });
      });

      this.data = Object.values(this.data);
    },
    clicked(d) {
      this.$emit('showPokemons', {names: d.pokemons, type: d.type});
    }
  }
}
</script>

<style lang="scss">
#chart {
  circle {
    cursor: pointer;
  }
}
</style>
