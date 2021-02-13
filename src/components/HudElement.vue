<template>
  <div
    class="hud-element"
    :class="outerElementClasses"
    :style="{
      '--background-color': backgroundColor,
      '--color': color,
      '--status': val
    }"
  >
    <div
      class="hud-icon"
      :style="{
        backgroundImage: `url('${icon}')`
      }"
    >
      <div class="hud-icon-indicator"></div>
    </div>
    <div class="hud-indicator-outer">
      <div class="hud-indicator-inner" :class="danger">
        <span class="hud-money-val" v-if="name === 'money'">{{ value }}</span>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "HudElement",
  props: {
    // UI
    state: String,

    // VAL
    value: Number,

    // CONF
    color: String,
    backgroundColor: String,

    // OTHER
    icon: String, // the icon is a string
    name: String
  },
  computed: {
    outerElementClasses() {
      let r = [];
      if (this.name === "money" && this.state !== "expanded") {
        r.push("hidden");
      } else {
        r.push(this.state);
      }
      return r;
    },
    val() {
      if (this.name === "money") {
        return 1;
      } else {
        return this.value;
      }
    },
    danger() {
      let r = [];
      if (this.name === "money") {
        if (this.value < 0) {
          r.push("danger-money");
        }
      } else {
        if (this.value <= 0.1) {
          r.push("danger");
        }
      }
      return r;
    }
  }
};
</script>
<style scoped lang="scss">
.hud-element {
  position: relative;
  --size: calc(5vh * var(--scale));

  --background-color: #4a4a55;
  --color: #000000;

  --status: 0.5;

  opacity: 1;

  transition: opacity 0.5s;

  background-color: var(--background-color);
  border-radius: calc(var(--size) / 2);
  .hud-icon {
    position: relative;
    overflow: hidden;

    height: var(--size);
    width: var(--size);

    background-size: auto 75%;
    background-position: center;
    background-repeat: no-repeat;
    border-radius: calc(var(--size) * 0.5);
    transition: background-color 0.5s, border 0.5s;
    .hud-icon-indicator {
      background-color: var(--color);
      transition: height 0.1s;
    }
  }
  .hud-indicator-outer {
    background-color: var(--background-color);
    --unit: calc(var(--size) * 0.6);
    --bar-length: var(--unit);
    position: absolute;
    z-index: 1;
    top: calc(var(--size) * 0.2);
    left: calc(var(--size) * 0.4);
    height: var(--unit);
    width: var(--bar-length);
    border-radius: calc(var(--unit) * 0.5);
    transition: width 0.5s, opacity 0.5s;
    .hud-indicator-inner {
      background-color: var(--color);
      height: var(--unit);
      width: var(--unit);
      border-radius: calc(var(--unit) * 0.5);
      transition: width 1s;
      text-align: center;
      color: white;
    }
  }
}
.danger-money {
  background-color: #cc3333 !important;
}
.danger {
  animation: danger 1s infinite;
}

@keyframes danger {
  0% {
    background-color: var(--color);
  }
  50% {
    background-color: #cc3333;
  }
  100% {
    background-color: var(--color);
  }
}

@import "../assets/scss/_elementHidden.scss";
@import "../assets/scss/_elementExpanded.scss";
@import "../assets/scss/_elementMinimized.scss";
</style>
