<template>
  <div ref="mapaContainer" class="mapa-container">
    <div class="svg-container">
      <svg ref="mapaSvg" :viewBox="viewBox">
        <g ref="mapGroup"></g>
      </svg>
      <div class="controls">
        <button @click="zoomIn">+</button>
        <button @click="zoomOut">-</button>
        <button @click="resetZoom">Reset zoom</button>
      </div>
    </div>
    <div v-if="tooltipVisible" class="tooltip" :style="{ left: tooltipPosition.x + 'px', top: tooltipPosition.y + 'px' }">
      <div class="tooltip-name">{{ tooltipName }}</div>
      <ul class="tooltip-payment-methods">
        <li v-for="(method, index) in tooltipPaymentMethods" :key="index">{{ formatPaymentMethod(method) }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import * as d3 from 'd3';
import * as topojson from 'topojson-client';

export default {
  data() {
    return {
      zoom: null,
      defaultColor: "#091321",
      hoverColor: "#3451ea",
      tooltipVisible: false,
      tooltipName: '',
      tooltipPaymentMethods: [],
      tooltipPosition: { x: 0, y: 0 },
      width: 0,
      height: 0,
      viewBox: '0 0 1000 500',
    };
  },
  mounted() {
    this.updateDimensions();
    this.crearMapa();
    window.addEventListener('resize', this.updateDimensions);
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.updateDimensions);
  },
  methods: {
    updateDimensions() {
      const container = this.$refs.mapaContainer;
      this.width = container.clientWidth;
      this.height = container.clientHeight;

      const scale = Math.min(this.width / 1000, this.height / 500);
      this.viewBox = `0 0 ${1000 * scale} ${500 * scale}`;

      d3.select(this.$refs.mapaSvg)
        .attr('width', this.width)
        .attr('height', this.height);

      if (this.zoom) {
        this.zoom.translateExtent([[-this.width, -this.height], [2 * this.width, 2 * this.height]]);
      }

      this.crearMapa();
    },
    async crearMapa() {
      const isMobile = window.innerWidth <= 768;
      const scale = isMobile
        ? Math.min(this.width / 1.97, this.height)
        : Math.min(this.width, this.height / 1.95);

      const projection = d3.geoMercator()
        .center([0, 0])
        .scale(scale)
        .translate([this.width / 2, this.height / 2]);

      const path = d3.geoPath().projection(projection);

      const svg = d3.select(this.$refs.mapaSvg);
      const g = d3.select(this.$refs.mapGroup);

      const topology = await d3.json("world-110m2.json");
      const countries = topojson.feature(topology, topology.objects.countries).features;

      const defaultColor = this.defaultColor;
      const hoverColor = this.hoverColor;

      g.selectAll("path")
        .data(countries)
        .enter()
        .append("path")
        .attr("d", path)
        .style("fill", (d) => d.properties.color || defaultColor)
        .style("stroke", "white")
        .style("stroke-width", "0.9px")
        .on("mouseover", (event, d) => {
          if (d.properties.paymentMethods && d.properties.paymentMethods.length > 0) {
            d3.select(event.target)
              .transition()
              .duration(300)
              .style("fill", hoverColor);

            this.tooltipName = d.properties.name;
            this.tooltipPaymentMethods = d.properties.paymentMethods[0].split(' '); // Convertir la cadena a un array
            this.tooltipVisible = true;
          }
        })
        .on("mousemove", (event) => {
          this.tooltipPosition = {
            x: event.clientX + 10,
            y: event.clientY + 10
          };
        })
        .on("mouseout", (event, d) => {
          d3.select(event.target)
            .transition()
            .duration(200)
            .style("fill", d.properties.color || defaultColor);

          this.tooltipVisible = false;
        });

      this.zoom = d3.zoom()
        .scaleExtent([1, 10])
        .translateExtent([[-this.width, -this.height], [2 * this.width, 2 * this.height]])
        .on("zoom", (event) => {
          d3.select(this.$refs.mapGroup).attr("transform", event.transform);
        });

      d3.select(this.$refs.mapaSvg)
        .call(this.zoom)
        .on("wheel.zoom", null)
        .on("dblclick.zoom", null);
    },
    zoomIn() {
      const svg = d3.select(this.$refs.mapaSvg);
      svg.transition().duration(750).call(this.zoom.scaleBy, 2);
    },
    zoomOut() {
      const svg = d3.select(this.$refs.mapaSvg);
      svg.transition().duration(750).call(this.zoom.scaleBy, 0.5);
    },
    resetZoom() {
      const svg = d3.select(this.$refs.mapaSvg);
      svg.transition().duration(750).call(this.zoom.transform, d3.zoomIdentity);
    },
    formatPaymentMethod(method) {
      // Convierte el método de pago a formato capitalizado
      return method.split(/(?=[A-Z])/).join(' ');
    }
  }
};
</script>

<style>
.mapa-container {
  position: relative;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0;
  padding: 0;
  overflow: hidden;
}

.svg-container {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

svg {
  display: block;
  max-width: 100%;
  max-height: 100%;
}

.controls {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 1000;
  display: flex;
  flex-direction: column;
}

.controls button {
  margin-bottom: 5px;
  padding: 10px;
  font-size: 16px;
}

.tooltip {
  position: absolute;
  background: white;
  padding: 10px;
  border-radius: 5px;
  pointer-events: none;
  z-index: 1000;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5);
}

.tooltip-name {
  color: #00214f;
  padding: 5px;
  border-radius: 3px 3px 0 0;
  margin-bottom: 5px;
  font-weight: 700;
  font-size: 20px;
}

.tooltip-payment-methods {
  padding: 5px;
  border-radius: 0 0 3px 3px;
  list-style-type: none;
  margin: 0;
  padding: 0;
}

.tooltip-payment-methods li {
  margin-bottom: 5px;
  color: black;
}

/* Media Queries */
@media (max-width: 768px) {
  .mapa-container {
    height: 100vh;
    width: 100%;
  }

  .controls {
    bottom: 10px;
    left: 10px;
    transform: none;
    display: flex;
    flex-direction: row;
  }

  .controls button {
    margin-right: 5px;
    font-size: 14px;
    margin-bottom: 5px;
    color: white;
    background-color: #0072f7;
    opacity: 0.6;
    margin-left: 5px;
    border: black 2px solid;
    border-radius: 10px;
  }
}

@media (min-width: 769px) {
  .mapa-container {
    height: 100vh;
    width: 100%;
  }

  .controls {
    bottom: 20px;
    transform: none;
    display: flex;
    flex-direction: row;
  }

  .controls button {
    margin-bottom: 5px;
    color: white;
    font-size: 16px;
    background-color: #0072f7;
    opacity: 0.6;
    margin-left: 5px;
    border: black 2px solid;
    border-radius: 10px;
    font-style: bold;
  }
}
</style>
