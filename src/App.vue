<template>
  <div id="app">
    <div class="parent-1">
      <div class="parent">
        <div class="block"></div>
      </div>
    </div>
    <button class="test" @click="visiblePiece(block)">
      <p>Рассчитать</p>
    </button>
    <p class="percent">{{visiblePercent}} %</p>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      block: null,
      mainParent: null,
      visiblePercent: '???'
    };
  },
  mounted() {
    this.block = document.querySelector(".block"); /// блок, у которого рассчитывается процент видимости
  },

  methods: {
    visiblePiece(block) {
      this.checkParent(block);
      this.calculateVisible(
        0,
        document.documentElement.clientHeight,
        0,
        document.documentElement.clientWidth,
        this.mainParent
      );
    },
    checkParent(block) {
      if (
        block.parentNode &&
        block.parentNode !== document.querySelector("#app")
      ) {
        const parent = block.parentNode;
        this.checkParent(parent);
      } else {
        this.mainParent = block;
      }
    },

    calculateVisible(top, bottom, left, right, elem) {
      const coordinates = elem.getBoundingClientRect();
      const coordinatesVisible = {
        top: null,
        bottom: null,
        left: null,
        right: null,
      };
      let elemVisibleY = null;
      if (coordinates.top < top && coordinates.bottom > bottom) {
        elemVisibleY = bottom - top;
        coordinatesVisible.top = top;
        coordinatesVisible.bottom = bottom;
      }
      if (coordinates.top < top && coordinates.bottom > bottom) {
        elemVisibleY = bottom - top;
      }
      if (coordinates.top >= top && coordinates.bottom <= bottom) {
        elemVisibleY = coordinates.height;
        coordinatesVisible.top = coordinates.top;
        coordinatesVisible.bottom = coordinates.bottom;
      }
      if (coordinates.top < top && bottom >= coordinates.bottom) {
        elemVisibleY = coordinates.bottom - top;
        coordinatesVisible.top = top;
        coordinatesVisible.bottom = coordinates.bottom;
      }
      if (coordinates.bottom > bottom && top <= coordinates.top) {
        elemVisibleY = bottom - coordinates.top;
        coordinatesVisible.top = coordinates.top;
        coordinatesVisible.bottom = bottom;
      }
      if (
        coordinates.bottom < top ||
        coordinates.top > bottom ||
        elemVisibleY < 0
      ) {
        elemVisibleY = 0;
      }

      let elemVisibleX = null;
      if (coordinates.left < left && coordinates.right > right) {
        elemVisibleX = right - left;
        coordinatesVisible.right = right;
        coordinatesVisible.left = left;
      }
      if (coordinates.left >= left && coordinates.right <= right) {
        elemVisibleX = coordinates.width;
        coordinatesVisible.left = coordinates.left;
        coordinatesVisible.right = coordinates.right;
      }
      if (coordinates.left < left && right >= coordinates.right) {
        elemVisibleX = coordinates.right - left;
        coordinatesVisible.left = left;
        coordinatesVisible.right = coordinates.right;
      }
      if (coordinates.right > right && left <= coordinates.left) {
        elemVisibleX = right - coordinates.left;
        coordinatesVisible.left = coordinates.left;
        coordinatesVisible.right = right;
      }
      if (
        coordinates.right < left ||
        coordinates.left > right ||
        elemVisibleX < 0
      ) {
        elemVisibleX = 0;
      }

      if (elem.childNodes.length !== 0 && elem !== this.block) {
        this.calculateVisible(
          coordinatesVisible.top,
          coordinatesVisible.bottom,
          coordinatesVisible.left,
          coordinatesVisible.right,
          elem.firstElementChild
        );
      } else {
        const fullElemArea = coordinates.width * coordinates.height;
        const visibleElemArea = Math.round(
          (elemVisibleX * elemVisibleY * 100) / fullElemArea * 10
        ) / 10;
        this.visiblePercent = visibleElemArea
      }
    },
  },
};
</script>

<style>
.div {
  height: 400px;
  width: 400px;
  background-color: #42b983;
  margin-top: 10px;
}
body {
  margin: 0px;
  height: 2500px;
  width: 3000px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

.parent {
  width: 500px;
  height: 400px;
  position: absolute;
  top: 150px;
  left: 100px;
  background-color: red;
}
.parent-1 {
  width: 700px;
  height: 600px;
  position: absolute;
  top: 350px;
  left: 300px;
  background-color: green;
}
.block {
  width: 300px;
  height: 300px;
  background-color: blue;
  position: absolute;
  top: 80px;
  left: 80px;
}
.test {
  position: fixed;
  left: 50%;
  top: 100px;
  font-size: 25px;
}
.percent {
  position: fixed;
  left: 50px;
  top: 50px;
  font-size: 25px;
}
</style>
