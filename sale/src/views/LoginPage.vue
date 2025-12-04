<template>
  <div
    class="login-page min-h-screen flex items-center justify-center bg-cover bg-center relative"
    :style="{ backgroundImage: `url(${backgroundImage})` }"
  >
    <!-- 背景遮罩：压暗一点提高对比度 -->
    <div class="absolute inset-0 bg-black bg-opacity-40"></div>

    <!-- 萤火虫画布：遮罩之上、卡片之下 -->
    <canvas ref="bgCanvas" class="login-bg-canvas"></canvas>

    <!-- 登录卡片 -->
    <div class="bg-white rounded-lg shadow-xl p-8 w-full max-w-md relative z-10 login-card">
      <div class="text-center mb-8">
        <h1 class="text-2xl font-bold text-[#007029]">融销通</h1>
        <p class="text-gray-600 mt-2">助力农业发展，共创美好未来</p>
      </div>

      <!-- 账号输入 -->
      <div class="mb-4">
        <label for="username" class="block text-sm font-medium text-gray-700 mb-1">账号</label>
        <div class="relative">
          <span class="absolute inset-y-0 left-0 flex items-center pl-3 text-gray-500">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
              <path
                fill-rule="evenodd"
                d="M10 9a3 3 0 100-6 3 3 0 000 6zm-7 9a7 7 0 1114 0H3z"
                clip-rule="evenodd"
              />
            </svg>
          </span>
          <input
            id="username"
            v-model="username"
            type="text"
            class="pl-10 w-full py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-[#007029] focus:border-transparent"
            placeholder="请输入账号"
            required
          />
        </div>
      </div>

      <!-- 密码输入 -->
      <div class="mb-4">
        <label for="password" class="block text-sm font-medium text-gray-700 mb-1">密码</label>
        <div class="relative">
          <span class="absolute inset-y-0 left-0 flex items-center pl-3 text-gray-500">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
              <path
                fill-rule="evenodd"
                d="M5 9V7a5 5 0 0110 0v2a2 2 0 012 2v5a2 2 0 01-2 2H5a2 2 0 01-2-2v-5a2 2 0 012-2zm8-2v2H7V7a3 3 0 016 0z"
                clip-rule="evenodd"
              />
            </svg>
          </span>
          <input
            id="password"
            v-model="password"
            :type="showPassword ? 'text' : 'password'"
            class="pl-10 w-full py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-[#007029] focus:border-transparent"
            placeholder="请输入密码"
            required
          />
          <button
            type="button"
            @click="togglePasswordVisibility"
            class="absolute inset-y-0 right-0 flex items-center pr-3 text-gray-500"
          >
            <svg
              v-if="showPassword"
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5"
              viewBox="0 0 20 20"
              fill="currentColor"
            >
              <path d="M10 12a2 2 0 100-4 2 2 0 000 4z" />
              <path
                fill-rule="evenodd"
                d="M.458 10C1.732 5.943 5.522 3 10 3s8.268 2.943 9.542 7c-1.274 4.057-5.064 7-9.542 7S1.732 14.057.458 10zM14 10a4 4 0 11-8 0 4 4 0 018 0z"
                clip-rule="evenodd"
              />
            </svg>
            <svg
              v-else
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5"
              viewBox="0 0 20 20"
              fill="currentColor"
            >
              <path
                fill-rule="evenodd"
                d="M3.707 2.293a1 1 0 00-1.414 1.414l14 14a1 1 0 001.414-1.414l-1.473-1.473A10.014 10.014 0 0019.542 10C18.268 5.943 14.478 3 10 3a9.958 9.958 0 00-4.512 1.074l-1.78-1.781zm4.261 4.26l1.514 1.515a2.003 2.003 0 012.45 2.45l1.514 1.514a4 4 0 00-5.478-5.478z"
                clip-rule="evenodd"
              />
              <path
                d="M12.454 16.697L9.75 13.992a4 4 0 01-3.742-3.741L2.335 6.578A9.98 9.98 0 00.458 10c1.274 4.057 5.065 7 9.542 7 .847 0 1.669-.105 2.454-.303z"
              />
            </svg>
          </button>
        </div>
      </div>

      <!-- 记住密码 -->
      <div class="flex items-center mb-6">
        <input
          id="remember"
          v-model="rememberMe"
          type="checkbox"
          class="h-4 w-4 text-[#007029] focus:ring-[#007029] border-gray-300 rounded"
        />
        <label for="remember" class="ml-2 block text-sm text-gray-700">记住密码</label>
      </div>

      <!-- 登录按钮 -->
      <button
        @click="userLogin()"
        class="w-full bg-[#007029] hover:bg-[#005d22] text-white py-2 px-4 rounded-md transition duration-300 flex items-center justify-center"
      >
        <span>登录</span>
      </button>

      <!-- 注册链接 -->
      <div class="text-center mt-6">
        <p class="text-sm text-gray-600">
          没有账号?
          <router-link to="/register" class="text-[#007029] hover:underline font-medium">
            立即注册
          </router-link>
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import riceImg from '@/assets/img/rice.png';
import { apiClient } from '../api/apiService.js';
import { ElMessage } from 'element-plus';
import { useStore } from 'vuex';
import { useRouter } from 'vue-router';
import { Base64 } from 'js-base64';

const store = useStore();
const router = useRouter();

// 背景图片
const backgroundImage = riceImg;

// 表单数据
const username = ref('');
const password = ref('');
const rememberMe = ref(false);
const showPassword = ref(false);

const togglePasswordVisibility = () => {
  showPassword.value = !showPassword.value;
};

// ==================== 多色萤火虫 + 鼠标吹散 ====================
const bgCanvas = ref(null);
const ctx = ref(null);
const fireflies = ref([]);
let animationId = null;

// 鼠标位置（用于“吹散”效果）
let mouseX = null;
let mouseY = null;

// 创建萤火虫
const createFireflies = (count) => {
  const canvas = bgCanvas.value;
  if (!canvas) return;
  const width = canvas.width;
  const height = canvas.height;

  fireflies.value = [];
  for (let i = 0; i < count; i++) {
    fireflies.value.push({
      x: Math.random() * width,
      y: Math.random() * height,

      // 大小&速度
      radius: 2 + Math.random() * 3.5,
      speed: 0.25 + Math.random() * 0.55,
      direction: Math.random() * Math.PI * 2,

      // 亮度&闪烁
      baseAlpha: 0.55 + Math.random() * 0.35,
      flickerSpeed: 0.0011 + Math.random() * 0.0012,
      offset: Math.random() * Math.PI * 2,

      // 颜色类型：0 黄绿、1 青绿、2 浅橙
      palette: Math.floor(Math.random() * 5),
    });
  }
};

// 绘制萤火虫
const drawFireflies = (time) => {
  const canvas = bgCanvas.value;
  const context = ctx.value;
  if (!canvas || !context) return;

  const width = canvas.width;
  const height = canvas.height;

  context.clearRect(0, 0, width, height);

  fireflies.value.forEach((f) => {
    // 正常漂浮：方向轻微扰动
    f.direction += (Math.random() - 0.5) * 0.03;

    f.x += Math.cos(f.direction) * f.speed;
    f.y += Math.sin(f.direction) * f.speed * 0.7;

    // 越界从另一侧回来
    if (f.x < -20) f.x = width + 20;
    if (f.x > width + 20) f.x = -20;
    if (f.y < -20) f.y = height + 20;
    if (f.y > height + 20) f.y = -20;

    // 若鼠标存在：做一个“吹散”效果
    if (mouseX !== null && mouseY !== null) {
      const dx = f.x - mouseX;
      const dy = f.y - mouseY;
      const dist2 = dx * dx + dy * dy;
      const blowRadius = 220; // 吹风半径
      const blowRadius2 = blowRadius * blowRadius;

      if (dist2 < blowRadius2) {
        const dist = Math.sqrt(dist2) || 1;
        const force = (blowRadius - dist) / blowRadius; // 0~1

        // 被风从鼠标处向外吹开，稍微向上偏一点
        const push = force * 14; // 力度
        f.x += (dx / dist) * push;
        f.y += (dy / dist) * push - force * 10;
      }
    }

    // 闪烁：亮度上下波动
    const flicker = Math.sin(time * f.flickerSpeed + f.offset) * 0.35;
    let alpha = f.baseAlpha + flicker;
    alpha = Math.max(0.15, Math.min(1, alpha));

    const glowRadius = f.radius * 4.8;
    const gradient = context.createRadialGradient(
      f.x,
      f.y,
      0,
      f.x,
      f.y,
      glowRadius
    );

    // 多色渐变
    if (f.palette === 0) {
  // ① 亮黄绿
  gradient.addColorStop(0, `rgba(235, 255, 200, ${alpha})`);
  gradient.addColorStop(0.35, `rgba(220, 255, 185, ${alpha * 0.9})`);
  gradient.addColorStop(0.7, `rgba(200, 240, 165, ${alpha * 0.55})`);
  gradient.addColorStop(1, `rgba(150, 210, 120, 0)`);

} else if (f.palette === 1) {
  // ② 薄荷青绿
  gradient.addColorStop(0, `rgba(210, 255, 245, ${alpha})`);
  gradient.addColorStop(0.35, `rgba(185, 245, 235, ${alpha * 0.9})`);
  gradient.addColorStop(0.7, `rgba(160, 230, 220, ${alpha * 0.55})`);
  gradient.addColorStop(1, `rgba(120, 190, 200, 0)`);

} else if (f.palette === 2) {
  // ③ 天空蓝
  gradient.addColorStop(0, `rgba(210, 230, 255, ${alpha})`);
  gradient.addColorStop(0.35, `rgba(185, 205, 250, ${alpha * 0.9})`);
  gradient.addColorStop(0.7, `rgba(150, 180, 240, ${alpha * 0.55})`);
  gradient.addColorStop(1, `rgba(110, 150, 220, 0)`);

} else if (f.palette === 3) {
  // ④ 樱花粉
  gradient.addColorStop(0, `rgba(255, 225, 235, ${alpha})`);
  gradient.addColorStop(0.35, `rgba(255, 205, 225, ${alpha * 0.9})`);
  gradient.addColorStop(0.7, `rgba(245, 180, 210, ${alpha * 0.55})`);
  gradient.addColorStop(1, `rgba(220, 150, 190, 0)`);

} else {
  // ⑤ 柔和紫
  gradient.addColorStop(0, `rgba(235, 225, 255, ${alpha})`);
  gradient.addColorStop(0.35, `rgba(220, 205, 250, ${alpha * 0.9})`);
  gradient.addColorStop(0.7, `rgba(195, 180, 240, ${alpha * 0.55})`);
  gradient.addColorStop(1, `rgba(160, 140, 220, 0)`);
}


    context.beginPath();
    context.fillStyle = gradient;
    context.arc(f.x, f.y, glowRadius, 0, Math.PI * 2);
    context.fill();
  });
};

// 画布大小
const resizeCanvas = () => {
  const canvas = bgCanvas.value;
  if (!canvas) return;
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;

  // 多色、亮一点，可以放多一些
  createFireflies(120);
};

// 鼠标移动：更新位置，用于“吹散”
const handleMouseMove = (e) => {
  const canvas = bgCanvas.value;
  if (!canvas) return;
  const rect = canvas.getBoundingClientRect();
  mouseX = e.clientX - rect.left;
  mouseY = e.clientY - rect.top;
};

// 鼠标移出窗口：停止影响
const handleMouseLeave = () => {
  mouseX = null;
  mouseY = null;
};

// 动画循环
const startAnimation = () => {
  const loop = (time) => {
    drawFireflies(time || 0);
    animationId = requestAnimationFrame(loop);
  };
  animationId = requestAnimationFrame(loop);
};

// 初始化
const initCanvas = () => {
  const canvas = bgCanvas.value;
  if (!canvas) return;
  ctx.value = canvas.getContext('2d');
  resizeCanvas();
  startAnimation();
};

onMounted(() => {
  initCanvas();
  window.addEventListener('resize', resizeCanvas);
  window.addEventListener('mousemove', handleMouseMove);
  window.addEventListener('mouseleave', handleMouseLeave);
});

onBeforeUnmount(() => {
  window.removeEventListener('resize', resizeCanvas);
  window.removeEventListener('mousemove', handleMouseMove);
  window.removeEventListener('mouseleave', handleMouseLeave);
  if (animationId) cancelAnimationFrame(animationId);
});

// ==================== 登录逻辑（原样保留） ====================
const param = ref({
  username: username.value,
  password: password.value,
});

const userLogin = async () => {
  try {
    param.value = {
      username: username.value,
      password: password.value,
    };
    const response = await apiClient.post(`user/login`, param.value, {
      headers: {
        'Content-Type': 'application/json',
        Authorization: window.localStorage.token,
      },
    });
    if (response.flag == true) {
      store.commit('setToken', response.data);

      const [, payload] = response.data.split('.');
      const decoded = Base64.decode(payload);
      const { nickname, avatar, role } = JSON.parse(decoded);

      store.commit('updateLoginUserNickname', nickname);
      store.commit('updateLoginUserAvatar', avatar);
      store.commit('updateRole', role);

      router.push('/home').catch((err) => err);
    } else {
      ElMessage.error(response.message);
    }
  } catch (error) {
    console.error('请求失败', error);
    throw error;
  }
};
</script>

<style>
body {
  font-family: 'PingFang SC', 'Microsoft YaHei', sans-serif;
}

/* 画布铺满全屏，允许点击透过去 */
.login-bg-canvas {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  display: block;
  z-index: 1;
  pointer-events: none;
  /* 让背景更亮一点、更有对比度，萤火虫更明显 */
  filter: brightness(1.25) contrast(1.1);
}

/* 登录卡片在最上层 */
.login-card {
  z-index: 2;
}

/* 夜间模式下卡片配合暗色主题 */
body.dark-mode .login-card {
  background: rgba(10, 14, 24, 0.92);
  color: #f5f5f5;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.85);
}
</style>
