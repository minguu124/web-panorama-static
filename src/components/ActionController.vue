<template>
  <div class="navbar">
    <ThumbCarousel
      :items="this.selectedGroup.items"
      :selected="this.selectedImage"
      :onChange="this.onChangeImage"
    />
    <div class="action">
      <button @click="this.onFullscreen" class="btn">
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
import ThumbCarousel from "./ThumbCarousel.vue";
export default {
  components: {
    ThumbCarousel,
  },
  methods: {
    onChangeImage(value) {
      this.selectedImage = value;
      this.onChangeImageAction(value);
    },
  },
  data() {
    return {
      selectedGroup: this.data["tong_quan"],
      selectedImage: this.data["tong_quan"].items[0]["360img"],
    };
  },
  props: {
    data: Object,
    onFullscreen: Function,
    onChangeImageAction: Function,
  },
  mounted() {},
};
</script>

<style>
.navbar {
  position: absolute;
  bottom: 16px;
  width: 50%;
  max-width: 720px;
  min-width: 360px;
  background-color: rgb(146, 100, 60, 0.7);
  left: 50%;
  transform: translate(-50%);
  flex-direction: column;
}

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
  flex: 0.3;
  text-align: center;
  vertical-align: middle;
  border: 1px solid #fff;
  aspect-ratio: 5/1;
  font-weight: 600;
  font-size: 14px;
  background-color: lightgrey;
  color: rgb(146, 100, 60);
}
.menu_button:hover {
  background-color: white;
}

.action {
  align-items: center;
  justify-content: space-around;
  padding: 16px;
  border-top: 2px solid burlywood;
}
</style>
