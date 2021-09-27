<template>
  <img
    class="container-map-div-marker"
    alt="marker"
    src="@/source/map/marker.png"
    :style="mData"
  />
</template>

<script>
export default {
  props: {
    mData: {
      type: Object,
    },
  },
};
</script>

<style></style>
