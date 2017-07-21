<template>
  <div class="system-graphic">

    <div class="graphic-content">
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
        <div v-if="tile.type" class="tile" v-for="tile in buildings" :style="buildingTileStyles(tile.pos)">
          <div class="dot-background"></div>
          <div class="dot" @click="showTooltip" :type="tile.type"></div>
        </div>
      </div>
    </div>

    <tooltip v-show="displayTooltip" @hideTooltip="hideTooltip" :content="tooltipContent"></tooltip>
  </div>
</template>

<script>
import Tooltip from './Tooltip';

export default {
  name: 'system-graphic',
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
        grid: require('../assets/0.png'),
        grass: require('../assets/grass.png'),
        stone: require('../assets/stone.png'),
        temple: require('../assets/temple.png'),
        factory: require('../assets/factory.png'),
        solar: require('../assets/solar.png'),
        house: require('../assets/house.png')
      }
    };
  },
  computed: {
    graphicStyles () {
      return {
        width: this.tileWidth * this.groundMap.length + 'px',
        height: this.tileHeight * (this.groundMap.length + 2) + 'px',
        padding: this.tileHeight + 'px'
      };
    },
    tileHeight () {
      return (this.tileWidth / this.tileRatio);
    },
    grid () {
      return this.ground.map(tile => Object.assign({}, tile, {type: 'grid'}));
    },
    ground () {
      return this.groundMap
        .map((row, y) => row
          .map((el, x) => {
            return {type: el, pos: [x, y]};
          })
        )
        .reduce((a, b) => a.concat(b), []);
    },
    buildings () {
      return this.buildingsMap
        .map((row, y) => row
          .map((el, x) => {
            return {type: el, pos: [x, y]};
          })
        )
        .reduce((a, b) => a.concat(b), []);
    }
  },
  methods: {
    groundTileStyles ([x, y]) {
      return {
        left: this.tileWidth / 2 * (this.groundMap.length + x - y - 1) + 'px',
        top: this.tileHeight / 2 * (x + y) + 'px',
        width: this.tileWidth + 'px'
      };
    },
    buildingTileStyles ([x, y]) {
      return {
        left: this.tileWidth / 2 * (this.groundMap.length + x - y - 1) + 'px',
        top: this.tileHeight / 2 * (x + y) - this.tileHeight + 'px',
        width: this.tileWidth + 'px',
        height: this.tileHeight * 2 + 'px'
      };
    },
    showTooltip (e) {
      this.displayTooltip = true;
      this.tooltipContent = e.target.getAttribute('type');
    },
    hideTooltip (e) {
      this.displayTooltip = false;
    }
  },
  mounted () {
    let graphic = document.querySelector('.system-graphic');
    let width = graphic.offsetWidth / (this.groundMap.length + 1);
    this.tileWidth = width;
    graphic.style.height = this.tileHeight * (this.groundMap.length + 1) + 'px';
    graphic.style.width = this.tileWidth * (this.groundMap.length + 1) + 'px';
    graphic.style.padding = this.tileHeight / 2 + 'px';
  }
};
</script>

<style>
  .system-graphic {}

  .graphic-content {
    position: relative;
  }

  .grid, .ground, .buildings, .markers {
    position: absolute;
  }

  .tile {
    position: absolute;
  }

  .grid-tile, .ground-tile, .building-tile, .marker-tile {
    width: 100%;
    height: 100%;
  }

  .dot, .dot-background {
    border-radius: 50%;
    width: 24px;
    height: 24px;
    position: absolute;
    top: 75%;
    left: 50%;
    transform: translate(-50%, -50%);
    opacity: 0.5;
    z-index: 2;
  }

  .dot {
    background: white;
  }

  .dot:hover {
    opacity: 1;
    transition: .3s ease-in;
    cursor: pointer;
  }

  .dot-background {
    border: 0.1em solid white;
    animation: pulse 3s 2s infinite;
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
      transform: scale(1.3) translate(-50%, -50%);
    }
    100% {
      opacity: 0;
      transform-origin: 0% 0%;
      transform: scale(1.6) translate(-50%, -50%);
    }
  }
</style>
