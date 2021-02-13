<template>
  <div id="hud-container" v-if="show">
    <HudElement
      v-for="e in elements"
      :key="e.key"
      :state="state"
      :value="e.value"
      :color="e.color"
      :backgroundColor="e.backgroundColor"
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
          value: 0,
          color: "#ffa500",
          backgroundColor: "#4a4a55",
          icon: require("./assets/icons/food.svg")
        },
        water: {
          key: "water",
          value: 0,
          color: "#6495ed",
          backgroundColor: "#4a4a55",
          icon: require("./assets/icons/water.svg")
        }
      },
      state: "expanded",
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
