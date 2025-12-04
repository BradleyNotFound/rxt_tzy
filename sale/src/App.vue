<template>
  <div id="app">
    <!-- 路由页面 -->
    <router-view />

    <!-- 返回顶部 -->
    <BackToTop />

    <!-- 字体大小调节按钮 -->
    <div class="font-size-switch">
      <button
        :class="{ active: fontSizeLevel === 'small' }"
        @click="setFontSize('small')"
      >
        A-
      </button>
      <button
        :class="{ active: fontSizeLevel === 'medium' }"
        @click="setFontSize('medium')"
      >
        A
      </button>
      <button
        :class="{ active: fontSizeLevel === 'large' }"
        @click="setFontSize('large')"
      >
        A+
      </button>
    </div>

    <!-- 夜间模式按钮 -->
    <div class="theme-switch">
      <input type="checkbox" id="theme-checkbox" v-model="isDark" />
      <label for="theme-checkbox" class="switch-label">
        <span class="sun">☀️</span>
        <span class="moon">🌙</span>
        <span class="ball"></span>
      </label>
    </div>
  </div>
</template>

<script>
import BackToTop from "./components/BackToTop.vue";

export default {
  components: { BackToTop },

  data() {
    return {
      isDark: false,
      fontSizeLevel: "medium", // small / medium / large
    };
  },

  mounted() {
    // 清空 localStorage
    window.onbeforeunload = function () {
      localStorage.clear();
    };

    // 恢复夜间模式
    const savedTheme = localStorage.getItem("theme");
    if (savedTheme === "dark") {
      this.isDark = true;
      document.body.classList.add("dark-mode");
    }

    // 恢复字体大小档位
    const savedSize = localStorage.getItem("fontSizeLevel");
    if (["small", "medium", "large"].includes(savedSize)) {
      this.fontSizeLevel = savedSize;
    }

    // 应用字体缩放
    this.applyFontSize();
  },

  watch: {
    isDark(newVal) {
      if (newVal) {
        document.body.classList.add("dark-mode");
        localStorage.setItem("theme", "dark");
      } else {
        document.body.classList.remove("dark-mode");
        localStorage.setItem("theme", "light");
      }
    },

    fontSizeLevel() {
      this.applyFontSize();
      localStorage.setItem("fontSizeLevel", this.fontSizeLevel);
    },
  },

  methods: {
    setFontSize(level) {
      this.fontSizeLevel = level;
    },

    // ★ 真正改变字体大小（不缩放网页，只改文字）
    applyFontSize() {
      document.documentElement.setAttribute(
        "data-font-size",
        this.fontSizeLevel
      );
    },
  },
};
</script>

<style lang="less">

/* ============================
   真正控制全站字体大小
   ⭐⭐ 关键：适配所有 px/rem/Tailwind 字体 ⭐⭐
============================ */
html {
  --font-base: 1;
  --font-adjust: 100%;
}

/* 小号字体：稍微小一点 */
html[data-font-size="small"] {
  --font-base: 0.95;
  --font-adjust: 98%;
}

/* 默认 */
html[data-font-size="medium"] {
  --font-base: 1;
  --font-adjust: 100%;
}

/* 大号字体：只放大一点点，不挤爆布局 */
html[data-font-size="large"] {
  --font-base: 1.08;
  --font-adjust: 104%;
}



/* ⭐ 核心：真正让所有文字变大或变小 ⭐
   不改变图片、不改变布局、不缩放元素
*/
/* ===============================
   字体缩放（不会覆盖颜色/背景）
================================= */

/* 全局文字缩放逻辑，只控制文字大小，不覆盖颜色 */
body,
body * {
  font-size: calc(var(--font-base) * 1em) !important;
  font-size-adjust: var(--font-adjust);
}

/* ⭐ 夜间模式保护层：重新把夜间模式的颜色盖回来 */
body.dark-mode,
body.dark-mode * {
  color: #f5f5f5 !important;
  text-shadow: 0 0 6px rgba(0,255,200,0.5);
}

/* 夜间模式下 bg-white 要变暗 */
body.dark-mode .bg-white {
  background-color: #121212 !important;
}

body.dark-mode header {
  background-color: #181818 !important;
  color: #f5f5f5 !important;
}

body.dark-mode main,
body.dark-mode section {
  background-color: #121212 !important;
}


/* ============================
   布局
============================ */
#app {
  width: 100%;
  height: 100%;
  background: #ffffff;
}

/* ============================
   字体调节按钮
============================ */
.font-size-switch {
  position: fixed;
  right: 20px;
  bottom: 880px;
  display: flex;
  gap: 6px;
  z-index: 9999;
}

.font-size-switch button {
  padding: 3px 6px;
  min-width: 32px;
  font-size: 12px;
  border-radius: 50px;
  border: 1px solid rgba(0,0,0,0.2);
  background: rgba(255,255,255,0.9);
  cursor: pointer;
  transition: 0.2s ease;
}

.font-size-switch button.active {
  background: #007029;
  color: white;
  box-shadow: 0 0 8px rgba(0,120,40,0.8);
}

body.dark-mode .font-size-switch button {
  background: rgba(0,0,0,0.5);
  color: #ddd;
  border-color: rgba(255,255,255,0.3);
}

body.dark-mode .font-size-switch button.active {
  background: #29ffb5;
  color: #000;
}

/* ============================
   夜间模式按钮
============================ */
.theme-switch {
  position: fixed;
  right: 20px;
  bottom: 20px;
  z-index: 9999;
}

.theme-switch input {
  display: none;
}

.switch-label {
  width: 60px;
  height: 30px;
  border-radius: 30px;
  background: linear-gradient(45deg,#ffd27d,#ffb347);
  display: flex;
  align-items: center;
  padding: 0 6px;
  position: relative;
  cursor: pointer;
}

.switch-label .ball {
  width: 26px;
  height: 26px;
  background: #fff;
  border-radius: 50%;
  position: absolute;
  left: 2px;
  top: 2px;
  transition: transform 0.3s ease;
}

input:checked + .switch-label {
  background: linear-gradient(45deg,#5a78ff,#1d1f38);
}

input:checked + .switch-label .ball {
  transform: translateX(30px);
}

/* ============================
   夜间模式 & 发光
============================ */
body.dark-mode {
  background: #121212;
  color: #f5f5f5;
}

body.dark-mode * {
  color: #f5f5f5 !important;
  text-shadow: 0 0 6px rgba(0,255,200,0.6);
}

/* 灯光柔光效果 */
body.dark-mode::after {
  content: "";
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 999;
  background:
    radial-gradient(circle at 1% 99%,rgba(255,230,180,0.55),transparent 11%),
    radial-gradient(circle at 98% 4%,rgba(255,240,200,0.40),transparent 9%);
}

</style>
