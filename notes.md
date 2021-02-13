# HUD Dev Notes - Do Not Include in Dist

## TODO

- remake the HUD but organized
- write the animations
- implement blinking if value is below .1, on the Money indicator it is going to be if its negative
- money indicator invisible if on minimized state

## Notes

### message types
```ts
type message: {
  type: "ui";
  state: "expanded" | "minimized" | "hidden";
  show: boolean;
} | {
  type: "val";
  food: number;         // Float 0-1
  water: number;        // Float 0-1
  money: number;        // -∞ - ∞
} | {
  type: "conf";
  colorFood: string;    // CSS Color Value
  colorWater: string;   // CSS Color Value
  colorMoney: string;   // CSS Color Value
  colorBg: string;      // CSS Color Value
  scale: number;
  opacity: number;      // Float 0-1
  gapScale: number;
}
```

### organisation

```html
<div class="hud-element">
  <div class="hud-icon">
    <div class="hud-icon-indicator"></div>
  </div>
  <div class="hud-indicator-outer">
    <div class="hud-indicator-inner"></div> 
  </div>
</div>
```

## Snippets for testing

### Testing UI messages ###
```js
// Messages for all states
postMessage ({
  type: "ui",
  state: "hidden",
  show: true
});
postMessage ({
  type: "ui",
  state: "expanded",
  show: true
});
postMessage ({
  type: "ui",
  state: "minimized",
  show: true
});

// Messages for hiding the UI (show: false)
postMessage ({
  type: "ui",
  state: "hidden",
  show: false
});
postMessage ({
  type: "ui",
  state: "expanded",
  show: false
});
postMessage ({
  type: "ui",
  state: "minimized",
  show: false
});
```

### Testing VAL messages ###
```js
postMessage({
  type: "val",
  food: 1,
  water: 1,
  money: 1000000000
});
postMessage({
  type: "val",
  food: 0.5,
  water: 0.5,
  money: 5000
});
postMessage({
  type: "val",
  food: 0,
  water: 0.1,
  money: -1000
});
```