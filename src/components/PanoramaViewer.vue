<template>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link
      rel="stylesheet"
      href="https://fonts.googleapis.com/icon?family=Material+Icons"
    />
  </head>
  <div class="viewer" id="viewer" />

  <div class="navbar">
    <ul class="carousel" id="carousel">
      <li v-for="item in items" :key="item.id" style="display: inline-block">
        <img
          v-bind:src="item.imageThumbnail"
          style="margin-right: 16px; border: 2px solid #fff; width: 200px"
        />
      </li>
    </ul>

    <div class="action">
      <button @click="handleExpand" class="btn">
        <i class="material-icons md-36">aspect_ratio</i>
      </button>
      <button @click="handleExpand" class="btn">
        <i class="material-icons md-36">expand_circle_down</i>
      </button>
      <div style="flex: 1; justify-content: space-around">
        <button
          class="menu_button"
          v-for="value in data"
          :key="value.label"
          v-on:click="items = value.data"
        >
          {{ value.label }}
        </button>
      </div>
      <button @click="handleExpand" class="btn">
        <i class="material-icons md-36">cameraswitch</i>
      </button>
      <button @click="handleExpand" class="btn">
        <i class="material-icons md-36">play_arrow</i>
      </button>
      <button class="btn">
        <i class="material-icons md-36">volume_up</i>
      </button>
    </div>
  </div>
</template>

<script>
import { Viewer } from "photo-sphere-viewer";
import "photo-sphere-viewer/dist/photo-sphere-viewer.css";
import { DATA } from "../constants/data";

export default {
  components: {},
  methods: {},
  data() {
    return {
      viewer: undefined,
      data: DATA,
      items: DATA["tong_quan"].items,
      handleExpand: () => {},
      carousel: {},
    };
  },
  mounted() {
    this.viewer = new Viewer({
      container: document.querySelector("#viewer"),
      plugins: [],
      panorama: this.items[0].image360,
      minFov: 50,
    });

    this.slider = document.querySelector("#carousel");

    this.slider.addEventListener("mousedown", (e) => {
      this.slider.classList.add("active");
      this.carousel = {
        ...this.carousel,
        isDown: true,
        startX: e.pageX - this.slider.offsetLeft,
        scrollLeft: this.slider.scrollLeft,
      };
    });
    this.slider.addEventListener("mouseleave", () => {
      this.carousel = {
        ...this.carousel,
        isDown: false,
      };
      this.slider.classList.remove("active");
    });
    this.slider.addEventListener("mouseup", () => {
      this.carousel = {
        ...this.carousel,
        isDown: false,
      };
      this.slider.classList.remove("active");
    });
    this.slider.addEventListener("mousemove", (e) => {
      if (!this.carousel.isDown) return;
      e.preventDefault();
      const x = e.pageX - this.slider.offsetLeft;
      const walk = (x - this.carousel.startX) * 3;
      this.slider.scrollLeft = this.carousel.scrollLeft - walk;
    });

    this.handleExpand = () => this.viewer.enterFullscreen();

    this.viewer.on("ready", () => {
      this.viewer.navbar.hide();
      this.viewer.renderer.camera.far *= 2;
      // new Animation({
      //   properties: {
      //     lat: { start: -Math.PI / 2, end: 0 },
      //     long: { start: Math.PI, end: 0 },
      //     zoom: { start: 0, end: 50 },
      //     fisheye: { start: 4, end: 0 },
      //   },
      //   duration: 2000,
      //   easing: "inOutQuad",
      //   onTick: (properties) => {
      //     this.viewer.setOption("fisheye", properties.fisheye);
      //     this.viewer.rotate({
      //       longitude: properties.long,
      //       latitude: properties.lat,
      //     });
      //     this.viewer.zoom(properties.zoom);
      //   },
      // });
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

.menu_button {
  display: inline-block;
  flex: 0.25;
  aspect-ratio: 4/1;
  text-align: center;
  vertical-align: middle;
  border: 1px solid #fff;
  font-weight: 600;
  font-size: 14px;
  background-color: lightgrey;
  color: rgb(146, 100, 60);
}
.menu_button:hover {
  background-color: white;
}

.viewer {
  width: 100vw;
  height: 100vh;
  background-color: aliceblue;
}
.navbar {
  position: absolute;
  bottom: 16px;
  width: 50%;
  max-width: 800px;
  background-color: rgb(146, 100, 60, 0.7);
  left: 50%;
  transform: translate(-50%);
  flex-direction: column;
}
.carousel {
  overflow-x: scroll;
  overflow-y: hidden;
  white-space: nowrap;
  transition: all 0.2s;
  transform: scale(0.98);
  will-change: transform;
  user-select: none;
  cursor: pointer;
}

.carousel.active {
  background: rgba(255, 255, 255, 0.3);
  cursor: grabbing;
  cursor: -webkit-grabbing;
  transform: scale(1);
}

.action {
  align-items: center;
  justify-content: space-around;
  flex: 0.5;
  padding: 0px 16px 16px;
}
</style>
