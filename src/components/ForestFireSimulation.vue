<template>
  <div id="forest-fire-simulation">
    <svg :width="width" :height="height"></svg>
  </div>
</template>

<script>
import * as d3 from 'd3';

export default {
  name: 'ForestFireSimulation',
  data() {
    return {
      width: 800,
      height: 600,
      grid: [],
      rows: 30,
      cols: 40,
      cellSize: 20,
      fireProbability: 0.01,
    };
  },
  mounted() {
    this.initializeGrid();
    this.startSimulation();
  },
  methods: {
    initializeGrid() {
      this.grid = Array.from({ length: this.rows }, () =>
        Array.from({ length: this.cols }, () => ({
          burning: Math.random() < this.fireProbability,
          burned: false,
        }))
      );
    },
    startSimulation() {
      d3.interval(() => {
        this.updateGrid();
        this.renderGrid();
      }, 500);
    },
    updateGrid() {
      const newGrid = this.grid.map((row, i) =>
        row.map((cell, j) => {
          if (cell.burning) {
            return { burning: false, burned: true };
          }
          const neighbors = [
            [i - 1, j],
            [i + 1, j],
            [i, j - 1],
            [i, j + 1],
          ];
          const burningNeighbor = neighbors.some(
            ([x, y]) =>
              x >= 0 &&
              x < this.rows &&
              y >= 0 &&
              y < this.cols &&
              this.grid[x][y].burning
          );
          return {
            burning: burningNeighbor && !cell.burned,
            burned: cell.burned,
          };
        })
      );
      this.grid = newGrid;
    },
    renderGrid() {
      const svg = d3.select('svg');
      const cells = svg.selectAll('rect').data(this.grid.flat());

      cells
        .enter()
        .append('rect')
        .merge(cells)
        .attr('x', (d, i) => (i % this.cols) * this.cellSize)
        .attr('y', (d, i) => Math.floor(i / this.cols) * this.cellSize)
        .attr('width', this.cellSize)
        .attr('height', this.cellSize)
        .attr('fill', (d) => (d.burning ? 'red' : d.burned ? 'black' : 'green'));

      cells.exit().remove();
    },
  },
};
</script>

<style scoped>
#forest-fire-simulation {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}
</style>