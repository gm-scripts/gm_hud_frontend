<template>
  <div
    id="hud-container"
    v-if="show"
    :style="{
      '--scale': scale,
      '--gapScale': gapScale,
      'opacity': opacity
    }"
    :class="position"
  >
    <HudElement
      v-for="e in elements"
      :key="e.key"
      :name="e.key"
      :state="state"
      :value="e.value"
      :color="e.color"
      :backgroundColor="backgroundColor"
      :icon="e.icon"
    ></HudElement>
  </div>
</template>

<script>
import HudElement from "./components/HudElement.vue";

export default {
  name: "App",
  components: {
    HudElement
  },
  data() {
    return {
      elements: {
        money: {
          key: "money",
          value: 1000,
          color: "#11bb42",
          icon: require("./assets/icons/dollar.svg")
        },
        food: {
          key: "food",
          value: 0.3,
          color: "#ffa500",
          icon: require("./assets/icons/food.svg")
        },
        water: {
          key: "water",
          value: 0.8,
          color: "#6495ed",
          icon: require("./assets/icons/water.svg")
        }
      },
      state: "minimized",
      opacity: 1,
      scale: 1,
      gapScale: 1,
      backgroundColor: "#4a4a55",
      // position: {
      //   x: 0,
      //   y: 0,
      //   leftOrRight: "left",
      //   topOrBottom: "top"
      // },
      show: true,
      pos: "bottom-left"
    };
  },
  computed: {
    position() {
      let r = [];
      r.push(this.pos);
      return r;
    }
  },
  mounted() {
    window.addEventListener("message", (event) => {
      let item = event.data;
      switch (item.type) {
        case "ui": {
          this.state = item.state;
          this.show = item.show;
          break;
        }
        case "val": {
          this.elements.food.value = item.food;
          this.elements.water.value = item.water;
          this.elements.money.value = item.money;
          break;
        }
        case "conf": {
          this.elements.food.color = item.colorFood;
          this.elements.water.color = item.colorWater;
          this.elements.money.color = item.colorMoney;

          this.backgroundColor = item.colorBg;
          this.opacity = item.opacity;
          this.scale = item.scale;
          this.gapScale = item.gapScale;

          this.pos = item.position;
          break;
        }
      }
    });
  }
};
</script>

<style lang="scss">
html,
body {
  // DEV
  // background-image: url("./assets/icons/Screenshot_3.png");
  // background-size: auto 100%;

  margin: 0;
  padding: 0;
  height: 100vh;
}
* {
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
#hud-container {
  position: absolute;
  --scale: 1;
  --gap-scale: 1;
  $gap: calc(3vh * var(--gapScale));
  display: grid;
  gap: $gap;
  padding: $gap;
}
.upper-left {
  left: 0;
}
.upper-right {
  right: 0;
}
.bottom-left {
  bottom: 0;
  left: 17vw;
}
</style>
