<template>
  <div class="container">
    <div>
      <h1>Check Map</h1>
      <div>dragging: {{ dragging }}</div>
      <div>img_x: {{ img_x }}</div>
      <div>img_y: {{ img_y }}</div>
      <div>elementOffsetX: {{ elementOffsetX }}</div>
      <div>elementOffsetY: {{ elementOffsetY }}</div>
      <div>mapHeight: {{ mapHeight }}</div>
      <div>mapWidth: {{ mapWidth }}</div>
    </div>
    <div id="mapBox" class="container-map">
      <div id="imgWrap" class="container-map-div">
        <img
          draggable="false"
          id="moveMap"
          class="container-map-div-piece"
          alt="map"
          src="@/source/map/map.png"
          @mousedown="dragStart"
          @mouseup="dragStop"
          @mousemove="dragGing"
          @contextmenu="marker"
        />
        <template v-for="m in markers">
          <CheckMarker :mData="m" :key="m.key" />
        </template>
      </div>
      <img
        class="container-map-reset"
        alt="reset"
        src="@/source/map/reset.png"
        @click="resetMarker"
      />
    </div>
  </div>
</template>

<script>
import CheckMarker from "@/components/Marker";

export default {
  components: {
    CheckMarker,
  },
  data() {
    return {
      markers: [],
      dragging: false,
      img_x: "no",
      img_y: "no",
      elementOffsetX: 0,
      elementOffsetY: 0,
      mapWidth: 0,
      mapHeight: 0,
      tisActive: true,
    };
  },
  methods: {
    resetMarker() {
      this.markers = [];
    },
    watchMap() {
      const d = document.querySelector("#moveMap");
      const wrap = document.querySelector("#imgWrap");
      wrap.style.width = `${d.clientWidth}px`;
      wrap.style.height = `${d.clientHeight}px`;
      this.mapWidth = d.clientWidth;
      this.mapHeight = d.clientHeight;
    },
    marker(e) {
      this.markers.push({
        left: `${e.layerX}px`,
        top: `${e.layerY}px`,
        key: this.markers.length,
      });
    },
    dragStart(e) {
      this.dragging = true;
      console.log("E", e);
      const box = document.querySelector("#imgWrap");
      this.elementOffsetX = this.img_x - box.offsetLeft;
      this.elementOffsetY = this.img_y - box.offsetTop;
      box.style.left = `${this.img_x - this.elementOffsetX}px`;
      box.style.top = `${this.img_y - this.elementOffsetY}px`;
    },
    dragStop() {
      this.dragging = false;
      this.img_x = this.img_y = "no";
    },
    dragGing(e) {
      this.img_x = e.clientX;
      this.img_y = e.clientY;

      let totalX = this.img_x - this.elementOffsetX;
      let totalY = this.img_y - this.elementOffsetY;
      if (this.dragging && totalX < 0 && totalY < 0) {
        const box = document.querySelector("#imgWrap");
        box.style.left = `${totalX}px`;
        box.style.top = `${totalY}px`;
      }
    },
  },
  mounted() {
    window.addEventListener("mouseup", this.dragStop);
    document.oncontextmenu = () => false;
    window.addEventListener("load", this.watchMap);
  },
  destroyed() {
    window.removeEventListener("load", this.watchMap);
  },
};
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  margin-bottom: 100px;
  &-map {
    display: block;
    position: relative;
    overflow: hidden;
    width: 1024px;
    height: 768px;
    cursor: move;
    // border: 1px solid black;
    &-div {
      display: block;
      position: absolute;
      z-index: 0;
      left: -200px;
      top: -200px;
      &-piece {
        position: absolute;
        z-index: 0;
        left: 0px;
        top: 0px;
        right: 0px;
        bottom: 0px;
      }
      &-marker {
        position: absolute;
        z-index: 2;
        cursor: pointer;
        transform: translate(-43%, -87%);
      }
    }
    &-reset {
      position: absolute;
      z-index: 3;
      right: 24px;
      top: 24px;
      cursor: pointer;
    }
  }
}
</style>
