<template>
  <div class="viewer" id="viewer" />
</template>

<script>
import { Viewer } from "photo-sphere-viewer";
import { MarkersPlugin } from "photo-sphere-viewer/dist/plugins/markers";
import "photo-sphere-viewer/dist/photo-sphere-viewer.css";
import "photo-sphere-viewer/dist/plugins/markers.css";

export default {
  expose: ["viewer"],
  props: {
    currentView: Object,
    onHotspotClick: Function,
  },
  watch: {
    currentView: function (newVal, prevVal) {
      if (newVal === prevVal) return;
      this.viewer.setPanorama(newVal.img360);
      this.marker.clearMarkers();
      newVal?.markers?.forEach((x) => {
        this.marker.addMarker(x);
      });
    },
  },
  data() {
    return {
      viewer: typeof Viewer,
      marker: undefined,
    };
  },
  mounted() {
    this.viewer = new Viewer({
      container: document.querySelector("#viewer"),
      panorama: this.currentView.img360,
      defaultLat: -0.5,
      defaultLong: 1.6,
      panoData: {
        fullWidth: 3840,
        fullHeight: 1920,
        croppedWidth: 3840,
        croppedHeight: 1920,
      },
      plugins: [
        [
          MarkersPlugin,
          {
            markers: this.currentView.markers,
          },
        ],
      ],
    });

    this.viewer.on("ready", () => {
      this.viewer.navbar.hide();
      this.marker = this.viewer.getPlugin(MarkersPlugin);

      this.marker.on("select-marker", (e, marker) => {
        this.onHotspotClick(marker.id);
      });
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
.marker {
  width: 8px;
  height: 8px;
  border-radius: 4px;
  background-color: #fff;
}
.marker-stick {
  width: 1px;
  height: 100px;
  background-color: #fff;
  position: absolute;
  bottom: 0px;
  right: 3.5px;
}
.marker-bubble {
  width: 90px;
  height: 90px;
  position: absolute;
  bottom: 90px;
  right: -44px;
  border-radius: 45px;
  border: 2px solid rgb(238, 170, 16);
  outline: 12px solid rgb(255, 255, 255, 0.4);
  overflow: hidden;
  transition: all 0.4s;
  transition-delay: 0.4s;
}
.marker-bubble:hover {
  width: 140px;
  height: 140px;
  bottom: 116px;
  right: -69px;
  border-radius: 70px;
  transition: all 0.4s;
  transition-delay: 0;
}
.marker-bubble-img {
  width: 100%;
  margin: 0;
}
.marker-label {
  padding: 4px 12px;
  position: absolute;
  width: max-content;
  transform: translateX(-50%);
  background-color: rgb(119, 46, 11);
  color: rgb(238, 170, 16);
  border-radius: 16px;
  font-weight: bold;
}
</style>
