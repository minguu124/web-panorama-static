<template>
  <main class="grid-item main">
    <div class="items">
      <div class="item item1"></div>
      <div class="item item2"></div>
      <div class="item item3"></div>
      <div class="item item4"></div>
      <div class="item item5"></div>
      <div class="item item6"></div>
      <div class="item item7"></div>
      <div class="item item8"></div>
      <div class="item item9"></div>
      <div class="item item10"></div>
    </div>
  </main>
</template>

<script>
export default {
  components: {},
  methods: {},
  data() {
    const slider = document.querySelector(".items");
    let isDown = false;
    let startX;
    let scrollLeft;

    slider.addEventListener("mousedown", (e) => {
      console.log("down");
      isDown = true;
      slider.classList.add("active");
      startX = e.pageX - slider.offsetLeft;
      scrollLeft = slider.scrollLeft;
    });
    slider.addEventListener("mouseleave", () => {
      isDown = false;
      slider.classList.remove("active");
    });
    slider.addEventListener("mouseup", () => {
      isDown = false;
      slider.classList.remove("active");
    });
    slider.addEventListener("mousemove", (e) => {
      if (!isDown) return;
      e.preventDefault();
      const x = e.pageX - slider.offsetLeft;
      const walk = (x - startX) * 3; //scroll-fast
      slider.scrollLeft = scrollLeft - walk;
      console.log(walk);
    });
  },
};
</script>

<style lang="scss">
$gray: #555;
$yellow: #f2e968;
$white: #efefef;
@supports (display: grid) {
  .grid-container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: auto 1fr auto;

    //Let the craziness begin!!!
    grid-template-areas:
      "header header header"
      "title title footer"
      "main main main";
    @media screen and (max-width: 500px) {
      grid-template-columns: 1fr;
      grid-template-rows: 0.3fr 1fr auto 1fr;
      grid-template-areas:
        "header"
        "title"
        "main"
        "footer";
    }
  }

  .grid-item {
    color: #fff;
    background: skyblue;
    padding: 3.5em 1em;
    font-size: 1em;
    font-weight: 700;
  }

  .header {
    background-color: darken(skyblue, 60%);
    grid-area: header;
    padding: 1em;
  }

  .title {
    color: $gray;
    background-color: lighten(skyblue, 25%);
    grid-area: title;
  }

  .main {
    color: lighten($gray, 25%);
    background-color: lighten(skyblue, 60%);
    grid-area: main;
    padding: 0;
    overflow-x: scroll;
    overflow-y: hidden;
  }

  .footer {
    background-color: darken(skyblue, 10%);
    grid-area: footer;
    padding: 0.6em;
  }

  .items {
    position: relative;
    width: 100%;
    overflow-x: scroll;
    overflow-y: hidden;
    white-space: nowrap;
    transition: all 0.2s;
    transform: scale(0.98);
    will-change: transform;
    user-select: none;
    cursor: pointer;
  }

  .items.active {
    background: rgba(255, 255, 255, 0.3);
    cursor: grabbing;
    cursor: -webkit-grabbing;
    transform: scale(1);
  }

  .item {
    display: inline-block;
    background: skyblue;
    min-height: 250px;
    min-width: 400px;
    margin: 2em 1em;
    @media screen and (max-width: 500px) {
      min-height: 200px;
      min-width: 200px;
    }
  }
}
</style>
