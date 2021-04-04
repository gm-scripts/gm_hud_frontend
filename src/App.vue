<template>
  <div
    id="hud-container"
    v-if="show"
    :style="{
      '--scale': scale,
      '--gap-scale': gapScale,
      '--bar-scale': barScale,
      opacity: opacity
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
      :active="e.active"
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
        time: {
          key: "time",
          value: this.time,
          color: "#9b4bbf",
          icon: require("./assets/icons/clock.svg"),
          active: true
        },
        money: {
          key: "money",
          value: "1000",
          color: "#11bb42",
          icon: require("./assets/icons/dollar.svg"),
          active: true
        },
        job: {
          key: "job",
          value: "LSPD: Chief of Police",
          color: "#904e3b",
          icon: require("./assets/icons/suitcase.svg"),
          active: true
        },
        food: {
          key: "food",
          value: "0.3",
          color: "#ffa500",
          icon: require("./assets/icons/food.svg"),
          active: true
        },
        water: {
          key: "water",
          value: "0.8",
          color: "#6495ed",
          icon: require("./assets/icons/water.svg"),
          active: true
        }
      },
      state: "minimized",
      opacity: 1,
      scale: 1,
      gapScale: 0.5,
      barScale: 7,
      backgroundColor: "#4a4a55",
      format: "h2:m2:s2 - D2/M2/Y4",
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
  methods: {
    // Dx => Day
    // Mx => Month
    // Yx => Year

    // sx => Second
    // mx => Minute
    // hx => Hour

    // x => minimum digits
    formatTime(format) {
      let r = "";
      const date = new Date();

      // DEV
      // console.log(format);

      for (let i = 0; i < format.length; i++) {
        switch (format[i]) {
          case "D": {
            let day = date.getDate();
            let outDay = day.toLocaleString("en-US", {
              minimumIntegerDigits: format[i + 1],
              useGrouping: false
            });
            i++;
            r += outDay;
            break;
          }
          case "M": {
            let day = date.getMonth() + 1;
            let outDay = day.toLocaleString("en-US", {
              minimumIntegerDigits: format[i + 1],
              useGrouping: false
            });
            i++;
            r += outDay;
            break;
          }
          case "Y": {
            let day = date.getFullYear();
            let outDay = day.toLocaleString("en-US", {
              minimumIntegerDigits: format[i + 1],
              useGrouping: false
            });
            i++;
            r += outDay;
            break;
          }
          case "s": {
            let day = date.getSeconds();
            let outDay = day.toLocaleString("en-US", {
              minimumIntegerDigits: format[i + 1],
              useGrouping: false
            });
            i++;
            r += outDay;
            break;
          }
          case "m": {
            let day = date.getMinutes();
            let outDay = day.toLocaleString("en-US", {
              minimumIntegerDigits: format[i + 1],
              useGrouping: false
            });
            i++;
            r += outDay;
            break;
          }
          case "h": {
            let day = date.getHours();
            let outDay = day.toLocaleString("en-US", {
              minimumIntegerDigits: format[i + 1],
              useGrouping: false
            });
            i++;
            r += outDay;
            break;
          }
          default: {
            r += format[i];
          }
        }
      }

      return r;
    }
  },
  mounted() {
    this.elements.time.value = this.formatTime(this.format);
    setInterval(() => {
      this.elements.time.value = this.formatTime(this.format);
    }, 1000);
    window.addEventListener("message", event => {
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
          this.elements.job.value = item.job;
          break;
        }
        case "conf": {
          this.elements.food.color = item.colorFood;
          this.elements.water.color = item.colorWater;
          this.elements.money.color = item.colorMoney;
          this.elements.job.color = item.colorJob;
          this.elements.time.color = item.colorTime;

          this.elements.food.active = item.foodActive;
          this.elements.water.active = item.waterActive;
          this.elements.money.active = item.moneyActive;
          this.elements.job.active = item.jobActive;
          this.elements.time.active = item.timeActive;
          this.format = item.timeFormat;

          this.backgroundColor = item.colorBg;
          this.opacity = item.opacity;
          this.scale = item.scale;
          this.gapScale = item.gapScale;
          this.barScale = item.barScale;

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
  overflow: hidden;
}
* {
  box-sizing: border-box;
  user-select: none;
}
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
#hud-container {
  position: absolute;
  --scale: 1;
  --gap-scale: 1;
  --bar-scale: 8;
  $gap: calc(3vh * var(--gap-scale));
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
