// ==UserScript==
// @name         Rainbow nickname
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  Made by Revenge Agma [gratefulprobe#1000]
// @author       Revenge Agma
// @match        https://agma.io/
// @run-at       document-start
// @grant        unsafeWindow
// ==/UserScript==

unsafeWindow.ytspec_colornick_color = "#FF0000";
unsafeWindow.ytspec_colornick_enabled = false;
unsafeWindow.ytspec_fakeskin_enabled = false;
unsafeWindow.ytspec_fakeskin_id = false;

function hslToHex(h, s, l) {
  h /= 360;
  s /= 100;
  l /= 100;
  let r, g, b;
  if (s === 0) {
    r = g = b = l; // achromatic
  } else {
    const hue2rgb = (p, q, t) => {
      if (t < 0) t += 1;
      if (t > 1) t -= 1;
      if (t < 1 / 6) return p + (q - p) * 6 * t;
      if (t < 1 / 2) return q;
      if (t < 2 / 3) return p + (q - p) * (2 / 3 - t) * 6;
      return p;
    };
    const q = l < 0.5 ? l * (1 + s) : l + s - l * s;
    const p = 2 * l - q;
    r = hue2rgb(p, q, h + 1 / 3);
    g = hue2rgb(p, q, h);
    b = hue2rgb(p, q, h - 1 / 3);
  }
  const toHex = x => {
    const hex = Math.round(x * 255).toString(16);
    return hex.length === 1 ? '0' + hex : hex;
  };
  return `#${toHex(r)}${toHex(g)}${toHex(b)}`;
}

class Settings {
    constructor(name, default_value) {
        this.settings = {};
        this.listeners = {
            load: [],
            save: [],
            set: [],
            get: []
        };
        this.default_value = default_value;
        this.name = name;
    }
    load() {
