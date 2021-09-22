<template>
  <div class="container">
    <div>
      <h1>Check Map</h1>
      <div>dragging: {{ dragging }}</div>
      <div>img_x: {{ img_x }}</div>
      <div>img_y: {{ img_y }}</div>
      <div>elementOffsetX: {{ elementOffsetX }}</div>
      <div>elementOffsetY: {{ elementOffsetY }}</div>
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
          @contextmenu="test"
        />
        <img
          class="container-map-div-marker"
          alt="marker"
          src="@/source/map/marker.png"
        />
      </div>
      <img
        class="container-map-reset"
        alt="reset"
        src="@/source/map/reset.png"
      />
      <!-- <img class="" alt="Vue logo" src="../assets/logo.png" /> -->
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dragging: false,
      img_x: "no",
      img_y: "no",
      elementOffsetX: 0,
      elementOffsetY: 0,
    };
  },
  methods: {
    test(e) {
      console.log(document.querySelector("#imgWrap").clientHeight);
      // document.querySelector("#imgWrap").clientHeight;
      console.log("aa", e);
    },
    dragStart() {
      this.dragging = true;
      // this.img_x = this.img_y = 0;
      const d = document.querySelector("#moveMap");
      this.elementOffsetX = this.img_x - d.offsetLeft;
      this.elementOffsetY = this.img_y - d.offsetTop;
      d.style.left = `${this.img_x - this.elementOffsetX}px`;
      d.style.top = `${this.img_y - this.elementOffsetY}px`;
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
        const d = document.querySelector("#moveMap");
        d.style.left = `${totalX}px`;
        d.style.top = `${totalY}px`;
      }
    },
  },
  mounted() {
    window.addEventListener("mouseup", this.dragStop);
    document.oncontextmenu = () => false;
  },
};
</script>

<style lang="scss">
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  border: 1px solid red;
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
      &-piece {
        position: absolute;
        z-index: 0;
        left: -200px;
        top: -200px;
        right: 0px;
        bottom: 0px;
      }
      &-marker {
        position: absolute;
        z-index: 2;
        left: 0px;
        top: 0px;
        //   border: 1px solid blue;
        cursor: pointer;
      }
    }
    &-reset {
      position: absolute;
      z-index: 3;
      right: 24px;
      top: 24px;
      cursor: pointer;
    }
    // background-image: url("../source/map/map.png");
  }
}
</style>
