<template>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css"
    />
  </head>
  <div
    id="viewer"
    style="width: 100vw; height: 100vh; background-color: aliceblue"
  ></div>
  <div
    style="
      position: absolute;
      bottom: 16px;
      width: 50%;
      aspect-ratio: 5/1;
      background-color: rgb(146, 100, 60, 0.6);
      left: 50%;
      transform: translate(-50%);
      flex-direction: column;
    "
  >
    <div style="flex: 1; align-items: center; justify-content: space-around">
      <img src="assets/thumbnail/TONG THE_1.jpg" />
      <img src="assets/thumbnail/TONG THE_2.jpg" />
      <img src="assets/thumbnail/TONG THE_3.jpg" />
    </div>
    <div style="align-items: center; justify-content: space-around; flex: 0.5">
      <button @click="handleExpand" class="btn">
        <i class="fa fa-expand"></i>
      </button>
      <button class="btn"><i class="fa fa-home"></i></button>
      <button class="btn"><i class="fa fa-home"></i></button>
    </div>
  </div>
</template>

<script>
import { Viewer, Animation } from "photo-sphere-viewer";
import "photo-sphere-viewer/dist/photo-sphere-viewer.css";
export default {
  components: {},
  methods: {},
  data() {
    return {
      viewer: undefined,
      navbar: undefined,
      handleExpand: () => {},
    };
  },
  mounted() {
    this.viewer = new Viewer({
      container: document.querySelector("#viewer"),
      panorama: "assets/panorama/default.jpg",
      plugins: [],
    });

    this.handleExpand = () => this.viewer.enterFullscreen();

    this.viewer.on("ready", () => {
      this.viewer.navbar.hide();
      this.viewer.renderer.camera.far *= 2;
      new Animation({
        properties: {
          lat: { start: -Math.PI / 2, end: 0 },
          long: { start: Math.PI, end: 0 },
          zoom: { start: 0, end: 50 },
          fisheye: { start: 4, end: 0 },
        },
        duration: 2000,
        easing: "inOutQuad",
        onTick: (properties) => {
          this.viewer.setOption("fisheye", properties.fisheye);
          this.viewer.rotate({
            longitude: properties.long,
            latitude: properties.lat,
          });
          this.viewer.zoom(properties.zoom);
        },
      });
    });
  },
};
</script>

<style>
.btn {
  background-color: transparent;
  border: 0;
  color: #fff;
  font-size: 24px;
}
img {
  width: 20%;
  margin-left: 4%;
}
</style>
