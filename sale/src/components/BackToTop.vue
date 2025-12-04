<template>
  <!-- 淡入淡出 -->
  <transition name="backtop-fade">
    <button
      v-if="visible"
      class="back-to-top"
      @click="scrollTop"
      aria-label="返回顶部"
    >
      ↑
    </button>
  </transition>
</template>

<script>
export default {
  name: "BackToTop",
  data() {
    return {
      visible: false,
    };
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.handleScroll);
  },
  methods: {
    handleScroll() {
      // 滚动超过 300px 才显示
      this.visible = window.pageYOffset > 300;
    },
    scrollTop() {
      window.scrollTo({
        top: 0,
        behavior: "smooth",
      });
    },
  },
};
</script>

<style>
.back-to-top {
  position: fixed;
  right: 30px;
  bottom: 130px; /* 刻意往上挪一点，避免挡住你的夜间模式按钮 */
  width: 50px;
  height: 50px;
  border-radius: 999px;
  border: none;
  outline: none;
  cursor: pointer;

  font-size: 20px;
  line-height: 1;
  font-weight: 600;

  background: rgba(202, 240, 191, 0.9);
  color: #333;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.18);

  display: flex;
  align-items: center;
  justify-content: center;

  backdrop-filter: blur(6px);
  transition: transform 0.2s ease, box-shadow 0.2s ease, background 0.2s ease,
    color 0.2s ease;
  z-index: 999;
}

.back-to-top:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.28);
}

/* 夜间模式下按钮也跟着“氛围感”一点 */
body.dark-mode .back-to-top {
  background: rgba(40, 40, 40, 0.92);
  color: #f8f8f8;
  box-shadow: 0 0 12px rgba(0, 255, 160, 0.5);
}

/* 出现 / 消失 的小动画 */
.backtop-fade-enter-active,
.backtop-fade-leave-active {
  transition: opacity 0.2s ease, transform 0.2s ease;
}

.backtop-fade-enter,
.backtop-fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>
