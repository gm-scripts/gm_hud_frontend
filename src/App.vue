<template>
  <div id="hud-container" v-if="show">
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
        },
        money: {
          key: "money",
          value: 1000,
          color: "#11bb42",
          icon: require("./assets/icons/dollar.svg")
        }
      },
      state: "minimized",
          backgroundColor: "#4a4a55",
      show: true
    };
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
          break;
        }
      }
    });
  }
};
</script>

<style lang="scss">
:root {
  --scale: 1;
  --gapScale: 1;
}
html,
body {
  // DEV
  background-image: url("./assets/icons/Screenshot_3.png");
  background-size: auto 100%;

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
  $gap: calc(3vh * var(--gapScale));
  display: grid;
  gap: $gap;
  padding: $gap;
}
</style>
