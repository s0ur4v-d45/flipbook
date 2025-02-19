<template>
  <div class="flipbook-container">
  
    <div class="flipbook">
      <div class="page cover front" @click="flipPage">Cover Page</div>
      <div class="page spread" @click="flipPage">
        <div class="left">Page 5</div>
        <div class="right">Page 6</div>
      </div>
      <div class="page spread" @click="flipPage">
        <div class="left">Page 3</div>
        <div class="right">Page 4</div>
      </div>
      <div class="page spread" @click="flipPage">
        <div class="left">Page 1</div>
        <div class="right">Page 2</div>
      </div>
      
    </div>
  </div>
</template>

<script>
export default {
  name: "FlipBook",
  data() {
    return {
      currentPage: 0
    };
  },
  methods: {
    flipPage(event) {
      const page = event.currentTarget;
      if (!page.classList.contains("flipped")) {
        page.classList.add("flipped");
      } else {
        page.classList.remove("flipped");
      }
    }
  }
};
</script>

<style>
.flipbook-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f0f0;
  position: relative;
}
.flipbook {
  display: flex;
  flex-direction: column;
  perspective: 1500px;
  position: relative;
  width: 320px;
  height: 450px;
}
.page {
  width: 100%;
  height: 100%;
  margin: 0;
  background: white;
  border: 1px solid #aaa;
  box-shadow: 3px 3px 10px rgba(0, 0, 0, 0.3);
  transition: transform 1s ease-in-out;
  transform-origin: left center;
  position: absolute;
  backface-visibility: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.2em;
  font-weight: bold;
}
.cover {
  background: linear-gradient(45deg, #ff7e5f, #feb47b);
  color: white;
  text-align: center;
  z-index: 5;
}
.spread {
  display: flex;
  flex-direction: row;
  z-index: 2;
}
.spread .left, .spread .right {
  flex: 1;
  text-align: center;
  line-height: 450px;
  border-right: 1px solid #ccc;
  background: white;
  box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.1);
}
.spread .right {
  border-right: none;
}
.flipped {
  transform: rotateY(-180deg);
  z-index: 1;
}
</style>

