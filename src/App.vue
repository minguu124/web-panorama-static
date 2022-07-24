<template>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link
      rel="stylesheet"
      href="https://fonts.googleapis.com/icon?family=Material+Icons"
    />
  </head>
  <div id="app">
    <PanoramaViewer ref="panoramaViewer" :currentView="this.selectedView" />
    <ActionController
      :data="this.data"
      :onFullscreen="this.onFullscreen"
      :onChangeImageAction="this.onChangeImageAction"
    />
  </div>
</template>

<script>
import PanoramaViewer from "@/components/PanoramaViewer.vue";
import ActionController from "@/components/ActionController.vue";
import { DATA } from "@/constants/data";

export default {
  name: "App",
  components: {
    PanoramaViewer,
    ActionController,
  },
  methods: {
    onChangeImageAction(value) {
      this.$refs.panoramaViewer.viewer.setPanorama(value["img360"]);
    },
  },
  data() {
    return {
      data: DATA,
      selectedView: DATA["tong_quan"].items[0].img360,
    };
  },
  mounted() {
    this.onFullscreen = () => {};
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body {
  margin: 0 !important;
}
div {
  display: flex;
}
</style>
