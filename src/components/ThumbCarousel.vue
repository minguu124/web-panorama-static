<template>
  <main>
    <button class="carousel_action"></button>
    <ul class="carousel" id="carousel">
      <li
        @click="onChange(item)"
        v-for="item in items"
        :key="item.id"
        class="carousel-item"
        :class="{ active: item.id === selected?.id }"
      >
        <img
          v-bind:src="item.imgThumb"
          class="carousel-item-image"
          :class="{ active: item.id === selected?.id }"
        />
        <div>{{ item.label }}</div>
      </li>
    </ul>
    <button class="carousel_action"></button>
  </main>
</template>

<script>
export default {
  props: {
    items: Array,
    selected: Object,
    onChange: Function,
  },
  data() {
    return {
      slider: undefined,
      carousel: {
        isDown: false,
      },
    };
  },
  mounted() {
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
  },
};
</script>

<style>
main {
  display: flex;
}

.carousel_action {
  background-color: transparent;
  border: 0px;
}
.carousel {
  flex: 1;
  overflow-x: scroll;
  overflow-y: hidden;
  white-space: nowrap;
  transition: all 0.5s;
  transform: scale(0.98);
  will-change: transform;
  user-select: none;
  cursor: pointer;
  -ms-overflow-style: none;
  scrollbar-width: none;
  padding: 0px 0px;
}

.carousel.active {
  cursor: grabbing;
  cursor: -webkit-grabbing;
  /* background: rgba(255, 255, 255, 0.3); */
  /* transform: scale(1); */
}

.carousel::-webkit-scrollbar {
  display: none;
}

.carousel-item {
  margin: 0 8px;
  width: 40%;
  display: inline-flex;
  font-size: 0px;
  padding: 0px;
  width: 20%;
  opacity: 0.6;
  justify-content: center;
  align-items: center;
}

.carousel-item.active {
  opacity: 1;
}

.carousel-item-image {
  width: 80%;
  /* transition: all 0.2s; */
  border: 2px solid #fff;
  margin: 0;
}

.carousel-item-image.active {
  width: 90%;
}
</style>
