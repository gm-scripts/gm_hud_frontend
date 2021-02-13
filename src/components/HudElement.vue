<template>
  <div class="hud-element" :class="outerElementClasses">
    <div class="hud-icon">
      <div class="hud-icon-indicator"></div>
    </div>
    <div class="hud-indicator-outer">
      <div class="hud-indicator-inner"></div>
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
    icon: String // the icon is a string
  },
  computed: {
    outerElementClasses() {
      let r = [];
      r.push(this.state);
      return r;
    }
  }
};
</script>
<style scoped lang="scss">
.hud-element {
  $size: calc(5vh * var(--scale));

  --background-color: #4a4a55;
  --color: #000000;

  opacity: 1;
  transform: scale(1);

  transition: opacity 0.5s, transform 0.5s;
  .hud-icon {
    height: $size;
    width: $size;

    background-color: var(--background-color);
    border-radius: calc($size / 2);
    .hud-icon-indicator {
      background-color: var(--color);
    }
  }
  .hud-indicator-outer {
    background-color: var(--background-color);
    .hud-indicator-inner {
      color: var(--color);
    }
  }
}
@import "../assets/scss/_elementHidden.scss";
@import "../assets/scss/_elementExpanded.scss";
@import "../assets/scss/_elementMinimized.scss";
</style>
