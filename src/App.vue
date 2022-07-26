<template>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link
      rel="stylesheet"
      href="https://fonts.googleapis.com/icon?family=Material+Icons"
    />
  </head>
  <div id="app">
    <PanoramaViewer
      ref="panoramaViewer"
      :currentView="this.selectedView"
      :onHotspotClick="this.onHotspotClick"
    />
    <ActionController
      :data="this.data"
      :currentGroup="this.selectedGroup"
      :currentView="this.selectedView"
      :onChangeGroup="this.onChangeGroup"
      :onChangeView="this.onChangeView"
      :onFullscreen="this.onFullscreen"
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
    onChangeGroup(group) {
      this.selectedGroup = group;
    },

    onChangeView(view) {
      this.selectedView = view;
    },

    onHotspotClick(id) {
      console.log(id);
      const items = JSON.parse(JSON.stringify(this.selectedGroup.items));
      const selected = items.find((x) => x.id === id);
      if (selected) {
        this.selectedView = selected;
      }
    },
  },
  data() {
    return {
      data: DATA,
      selectedGroup: DATA["tong_quan"],
      selectedView: DATA["tong_quan"].items[0],
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
html,
body {
  margin: 0 !important;
  width: 100vw;
  height: 100vh;
}
div {
  display: flex;
}
</style>
