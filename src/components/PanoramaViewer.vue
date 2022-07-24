<template>
  <div class="viewer" id="viewer" />
</template>

<script>
import { Viewer } from "photo-sphere-viewer";
import "photo-sphere-viewer/dist/photo-sphere-viewer.css";

export default {
  expose: ["viewer"],
  components: {},
  methods: {},
  props: {
    currentView: String,
  },
  watch: {
    currentView: function (newVal, prevVal) {
      if (newVal === prevVal) return;
      this.viewer.setPanorama(newVal);
    },
  },
  data() {
    return {
      viewer: typeof Viewer,
    };
  },
  mounted() {
    this.viewer = new Viewer({
      container: document.querySelector("#viewer"),
      panorama: this.currentView,
      plugins: [],
      minFov: 50,
    });
    this.viewer.on("ready", () => {
      this.viewer.navbar.hide();
    });
  },
};
</script>

<style>
.viewer {
  width: 100vw;
  height: 100vh;
  background-color: aliceblue;
}
</style>
