<template>
  <!-- 整个应用的外层容器 -->
  <div id="app">
    <!-- 路由页面 -->
    <router-view />

    <!-- 夜间模式切换按钮（固定在右下角） -->
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
export default {
  data() {
    return {
      isDark: false,
    };
  },

  mounted() {
    // 离开页面时清空 LocalStorage（你原来的逻辑）
    window.onbeforeunload = function () {
      const storage = window.localStorage;
      storage.clear();
    };

    // 页面加载时恢复夜间模式
    const savedTheme = localStorage.getItem("theme");
    if (savedTheme === "dark") {
      this.isDark = true;
      document.body.classList.add("dark-mode");
    }
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
  },
};
</script>

<style lang="less">
/* 你的原始样式 */
#app {
  height: 100%;
  background: #ffffff;
  width: 100%;
}

/* 整个首页在夜间变黑 */
body.dark-mode .home-page {
  background-color: #121212 !important;
}





/* ===================== 夜间模式按钮样式 ===================== */

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
  background: linear-gradient(45deg, #ffd27d, #ffb347);
  border-radius: 30px;
  position: relative;
  cursor: pointer;
  transition: background 0.3s ease;
  display: flex;
  align-items: center;
  padding: 0 6px;
}

.switch-label .sun,
.switch-label .moon {
  font-size: 16px;
}

.switch-label .ball {
  width: 26px;
  height: 26px;
  background: #ffffff;
  border-radius: 50%;
  position: absolute;
  top: 2px;
  left: 2px;
  transition: transform 0.35s ease;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.25);
}

input:checked + .switch-label .ball {
  transform: translateX(30px);
}

input:checked + .switch-label {
  background: linear-gradient(45deg, #5a78ff, #1d1f38);
}

/* ===================== 全局主题（亮/暗） ===================== */

body {
  background: #ffffff;
  color: #333333;
  transition: background 0.3s ease, color 0.3s ease;
}

body.dark-mode {
  background: #121212;
  color: #eeeeee;
}

/* 让 #app 配合变黑 */
body.dark-mode #app {
  background-color: #121212;
  color: #eeeeee;
}

/* ===================== ★★★★★ Tailwind 背景强制覆盖 ★★★★★ */

/* 所有 bg-white 的元素在夜间模式下全部变黑 */
body.dark-mode .bg-white {
  background-color: #121212 !important;
  color: #eeeeee !important;
}

/* min-h-screen 是你的页面最大容器，也要变黑 */
body.dark-mode .min-h-screen {
  background-color: #121212 !important;
}

/* header 通常使用 bg-white，我们额外调深一点 */
body.dark-mode header {
  background-color: #181818 !important;
  color: #eeeeee !important;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.6);
}

/* footer 的绿色保持，只调亮文字 */
body.dark-mode footer {
  color: #f5f5f5 !important;
}

/* 让所有页面的 main 区域在夜间变暗 */
body.dark-mode main {
  background-color: #121212 !important;
}

/* 保险起见，把 section 也一起调暗（不会影响上面那张大图） */
body.dark-mode section {
  background-color: #121212 !important;
}

/* 夜间模式：让常见文字都变亮 */
body.dark-mode,
body.dark-mode h1,
body.dark-mode h2,
body.dark-mode h3,
body.dark-mode h4,
body.dark-mode p,
body.dark-mode span,
body.dark-mode a,
body.dark-mode li,
body.dark-mode div {
  color: #f5f5f5 !important;
}

/* 覆盖 Tailwind 里那些偏灰的文字颜色 */
body.dark-mode .text-gray-500,
body.dark-mode .text-gray-600,
body.dark-mode .text-gray-700,
body.dark-mode .text-gray-800 {
  color: #f5f5f5 !important;
}
/* 夜间模式下：更明显的发光效果（淡绿色霓虹） */
body.dark-mode h1,
body.dark-mode h2,
body.dark-mode h3,
body.dark-mode p,
body.dark-mode span,
body.dark-mode a {
  text-shadow:
    0 0 4px rgba(0, 255, 200, 0.9),
    0 0 10px rgba(0, 200, 150, 0.7);
}



</style>
