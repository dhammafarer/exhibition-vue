<template>
  <div id="app">

    <div class="grid">
      <div class="row" v-for="(row, rowIndex) in grid" :style="rowPosition(rowIndex)">
        <div class="tile" v-for="(tile, tileIndex) in row" :style="tileStyles(tileIndex)">
          <img class="grid-tile" :src="tiles['zero']"/>
        </div>
      </div>
    </div>

    <div class="ground">
      <div class="row" v-for="(row, rowIndex) in grid" :style="rowPosition(rowIndex)">
        <div class="tile" v-for="(tile, tileIndex) in row" :style="tileStyles(tileIndex)">
          <img class="grid-tile" :src="tiles[tile]"/>
        </div>
      </div>
    </div>

    <div class="structures">
      <div class="row" v-for="(row, rowIndex) in objects" :style="rowPosition(rowIndex)">
        <div v-if="tile" class="tile" v-for="(tile, tileIndex) in row" :style="structureStyles(tileIndex)">
          <img class="structure-tile" :src="tiles[tile]"/>
        </div>
      </div>
    </div>

    <div class="markers">
      <div class="row" v-for="(row, rowIndex) in objects" :style="rowPosition(rowIndex)">
        <div v-if="tile" class="tile" v-for="(tile, tileIndex) in row" :style="tileStyles(tileIndex)">
          <div class="dot-background"></div>
          <div class="dot"></div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import Hello from './components/Hello'

export default {
  name: 'app',
  components: {
    Hello
  },
  data () {
    return {
      tileRatio: 1.7345,
      tileWidth: 200,
      offset: 0.94,
      grid: [
        ['grass', 'grass', 'grass'],
        ['grass', 'grass', 'grass'],
        ['grass', 'stone', 'grass']

      ],
      objects: [
        ['temple', null, 'solar'],
        [null, null, null],
        [null, 'factory', null]
      ],
      tiles: {
        zero: require('./assets/0.png'),
        grass: require('./assets/grass.png'),
        stone: require('./assets/stone.png'),
        temple: require('./assets/temple.png'),
        factory: require('./assets/factory.png'),
        solar: require('./assets/solar.png')
      }
    }
  },
  computed: {
    tileHeight () {
      return (this.tileWidth / this.tileRatio)
    }
  },
  methods: {
    rowPosition (i) {
      return {
        top: (this.tileHeight * this.offset) / 2 * i + 'px',
        left: this.tileWidth / 2 * (this.grid.length - i) + 'px'
      }
    },
    tilePosition (i) {
      return {
        top: (this.tileHeight * this.offset) / 2 * i + 'px',
        left: this.tileWidth / 2 * i + 'px'
      }
    },
    tileStyles (i) {
      return {
        width: this.tileWidth + 'px',
        height: this.tileHeight + 'px',
        top: this.tilePosition(i).top,
        left: this.tilePosition(i).left
      }
    },
    structureStyles (i) {
      return {
        width: this.tileWidth + 'px',
        height: this.tileHeight * 2 + 'px',
        top: this.tilePosition(i).top,
        left: this.tilePosition(i).left
      }
    }
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
  }

  #app {
    position: relative;
    margin-top: 120px;
  }

  .grid, .ground, .structures, .markers {
    position: absolute;
  }

  .tile {
    position: absolute;
  }

  .grid-tile, .ground-tile, .structure-tile {
    width: 100%;
    height: 100%;
  }

  .dot, .dot-background {
    border-radius: 50%;
    width: 1em;
    height: 1em;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    opacity: 0.8;
    z-index: 2;
  }

  .dot {
    background: white;
  }

  .dot:hover {
    opacity: 1;
    transition: .3s ease-in;
  }

  .dot-background {
    border: 2px solid white;
    animation: pulse 3s 2s infinite;
  }

  .row {
    position: absolute;
  }

  .structure-tile {
    transform: translateY(-50%);
    width: 100%;
    height: 100%;
  }

  @keyframes pulse {
    0% {
      opacity: 0.5;
      transform-origin: 0% 0%;
      transform: scale(1.1) translate(-50%, -50%);
    }
    100% {
      opacity: 0;
      transform-origin: 0% 0%;
      transform: scale(1.5) translate(-50%, -50%);
    }
  }
</style>
