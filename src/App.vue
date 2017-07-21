<template>
  <div id="app">

    <div class="graphic">

      <div v-if="show.grid" class="grid">
        <div class="tile" v-for="tile in ground" :style="groundTileStyles(tile.pos)">
          <img class="ground-tile" :src="tiles['grid']">
        </div>
      </div>

      <div v-if="show.ground" class="ground">
        <div class="tile" v-for="tile in ground" :style="groundTileStyles(tile.pos)">
          <img class="ground-tile" :src="tiles[tile.type]"/>
        </div>
      </div>

       <div v-if="show.buildings" class="buildings">
        <div v-if="tile.type" class="tile" v-for="tile in buildings" :style="buildingTileStyles(tile.pos)">
          <img class="ground-tile" :src="tiles[tile.type]"/>
        </div>
      </div>

       <div v-if="show.markers" class="markers">
        <div v-if="tile.type" class="tile" v-for="tile in buildings" :style="groundTileStyles(tile.pos)">
          <div class="dot-background"></div>
          <div class="dot"></div>
        </div>
      </div>

    </div>

    <tooltip v-show="displayTooltip" @hideTooltip="hideTooltip" :content="tooltipContent"></tooltip>
  </div>
</template>

<script>
import Tooltip from './components/Tooltip'

export default {
  name: 'app',
  components: {
    Tooltip
  },
  data () {
    return {
      show: {
        grid: true,
        ground: true,
        buildings: true,
        markers: true
      },
      displayTooltip: false,
      tooltipContent: 'text...',
      tileRatio: 1.7345,
      tileWidth: 200,
      groundMap: [
        ['grass', 'grass', 'grass'],
        ['grass', 'grass', 'grass'],
        ['stone', 'stone', 'grass']

      ],
      buildingsMap: [
        ['temple', null, 'solar'],
        ['house', null, 'house'],
        [null, 'factory', null]
      ],
      tiles: {
        grid: require('./assets/0.png'),
        grass: require('./assets/grass.png'),
        stone: require('./assets/stone.png'),
        temple: require('./assets/temple.png'),
        factory: require('./assets/factory.png'),
        solar: require('./assets/solar.png'),
        house: require('./assets/house.png')
      }
    }
  },
  computed: {
    tileHeight () {
      return (this.tileWidth / this.tileRatio)
    },
    grid () {
      return this.ground.map(tile => Object.assign({}, tile, {type: 'grid'}))
    },
    ground () {
      return this.groundMap
        .map((row, y) => row
          .map((el, x) => {
            return {type: el, pos: [x, y]}
          })
        )
        .reduce((a, b) => a.concat(b), [])
    },
    buildings () {
      return this.buildingsMap
        .map((row, y) => row
          .map((el, x) => {
            return {type: el, pos: [x, y]}
          })
        )
        .reduce((a, b) => a.concat(b), [])
    }
  },
  methods: {
    groundTileStyles ([x, y]) {
      return {
        left: this.tileWidth / 2 * (this.groundMap.length + x - y) + 'px',
        top: this.tileHeight / 2 * (x + y) + 'px',
        width: this.tileWidth + 'px'
      }
    },
    buildingTileStyles ([x, y]) {
      return {
        left: this.tileWidth / 2 * (this.groundMap.length + x - y) + 'px',
        top: this.tileHeight / 2 * (x + y) - this.tileHeight + 'px',
        width: this.tileWidth + 'px'
      }
    },
    showTooltip (e) {
      this.displayTooltip = true
      this.tooltipContent = e.target.getAttribute('type')
      console.log(e)
    },
    hideTooltip (e) {
      this.displayTooltip = false
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
  }

  .graphic {
    position: relative;
    margin-top: 60px;
  }

  .grid, .ground, .buildings, .markers {
    position: absolute;
  }

  .tile {
    position: absolute;
  }

  .grid-tile, .ground-tile, .building-tile {
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
    opacity: 0.5;
    z-index: 2;
  }

  .dot {
    background: red;
  }

  .dot:hover {
    opacity: 1;
    transition: .3s ease-in;
    cursor: pointer;
  }

  .dot-background {
    border: 2px solid white;
    animation: pulse 3s 2s infinite;
  }

  .row {
    position: absolute;
  }

  .building-tile {
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
