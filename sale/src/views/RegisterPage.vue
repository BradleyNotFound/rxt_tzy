<template>
  <div
    class="min-h-screen flex items-center justify-center bg-cover bg-center relative"
    :style="{ backgroundImage: `url(${backgroundImage})` }"
  >
    <!-- 背景遮罩 -->
    <div class="absolute inset-0 bg-black bg-opacity-40"></div>

    <!-- 萤火虫画布：遮罩之上、卡片之下 -->
    <canvas ref="bgCanvas" class="login-bg-canvas"></canvas>

    <!-- 注册卡片 -->
    <div
      class="bg-white rounded-lg shadow-xl p-8 w-full max-w-md relative z-10 login-card"
    >
      <div class="text-center mb-8">
        <h1 class="text-2xl font-bold text-[#007029]">融销通</h1>
        <p class="text-gray-600 mt-2">注册账号，开启农业服务</p>
      </div>

      <!-- 用户名输入 -->
      <div class="mb-4">
        <label
          for="username"
          class="block text-sm font-medium text-gray-700 mb-1"
          >用户名</label
        >
        <div class="relative">
          <span
            class="absolute inset-y-0 left-0 flex items-center pl-3 text-gray-500"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5"
              viewBox="0 0 20 20"
              fill="currentColor"
            >
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
            placeholder="请输入用户名（用于登录）"
            required
          />
        </div>
      </div>

      <!-- 昵称输入 -->
      <div class="mb-4">
        <label
          for="nickname"
          class="block text-sm font-medium text-gray-700 mb-1"
          >昵称</label
        >
        <div class="relative">
          <span
            class="absolute inset-y-0 left-0 flex items-center pl-3 text-gray-500"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5"
              viewBox="0 0 20 20"
              fill="currentColor"
            >
              <path
                fill-rule="evenodd"
                d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-6-3a2 2 0 11-4 0 2 2 0 014 0zm-2 4a5 5 0 00-4.546 2.916A5.986 5.986 0 005 10a6 6 0 0012 0c0-.35-.035-.691-.1-1.021A5 5 0 0010 11z"
                clip-rule="evenodd"
              />
            </svg>
          </span>
          <input
            id="nickname"
            v-model="nickname"
            type="text"
            class="pl-10 w-full py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-[#007029] focus:border-transparent"
            placeholder="请输入您的昵称"
            required
          />
        </div>
      </div>

      <!-- 密码输入 -->
      <div class="mb-4">
        <label
          for="password"
          class="block text-sm font-medium text-gray-700 mb-1"
          >密码</label
        >
        <div class="relative">
          <span
            class="absolute inset-y-0 left-0 flex items-center pl-3 text-gray-500"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5"
              viewBox="0 0 20 20"
              fill="currentColor"
            >
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
            placeholder="请设置密码"
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
        <p class="text-xs text-gray-500 mt-1">
          密码必须以字母开头，长度在6-18之间，只能包含英文字符、数字和下划线
        </p>
      </div>

      <!-- 确认密码输入 -->
      <div class="mb-6">
        <label
          for="confirmPassword"
          class="block text-sm font-medium text-gray-700 mb-1"
          >确认密码</label
        >
        <div class="relative">
          <span
            class="absolute inset-y-0 left-0 flex items-center pl-3 text-gray-500"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5"
              viewBox="0 0 20 20"
              fill="currentColor"
            >
              <path
                fill-rule="evenodd"
                d="M2.166 4.999A11.954 11.954 0 0010 1.944 11.954 11.954 0 0017.834 5c.11.65.166 1.32.166 2.001 0 5.225-3.34 9.67-8 11.317C5.34 16.67 2 12.225 2 7c0-.682.057-1.35.166-2.001zm11.541 3.708a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z"
                clip-rule="evenodd"
              />
            </svg>
          </span>
          <input
            id="confirmPassword"
            v-model="confirmPassword"
            :type="showConfirmPassword ? 'text' : 'password'"
            class="pl-10 w-full py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-[#007029] focus:border-transparent"
            placeholder="请再次输入密码"
            required
          />
          <button
            type="button"
            @click="toggleConfirmPasswordVisibility"
            class="absolute inset-y-0 right-0 flex items-center pr-3 text-gray-500"
          >
            <svg
              v-if="showConfirmPassword"
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

      <!-- 注册按钮 -->
      <button
        class="w-full bg-[#007029] hover:bg-[#005d22] text-white py-2 px-4 rounded-md transition duration-300 flex items-center justify-center"
        @click="handleRegister()"
      >
        <span>注册</span>
      </button>

      <!-- 登录链接 -->
      <div class="text-center mt-6">
        <p class="text-sm text-gray-600">
          已有账号?
          <router-link
            to="/login"
            class="text-[#007029] hover:underline font-medium"
            >点击登录</router-link
          >
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from "vue";
import { useRouter } from "vue-router";
import { ElNotification, ElMessage } from "element-plus";
import riceImg from "@/assets/img/rice.png";
import { apiClient } from "../api/apiService.js";

const router = useRouter();
// 背景图片
const backgroundImage = riceImg;

// 表单数据
const username = ref("");
const nickname = ref("");
const password = ref("");
const confirmPassword = ref("");
const showPassword = ref(false);
const showConfirmPassword = ref(false);

// 表单验证
const isFormValid = computed(() => {
  return (
    username.value.length >= 3 &&
    nickname.value.length >= 2 &&
    password.value.length >= 6 &&
    password.value === confirmPassword.value
  );
});

// 切换密码可见性
const togglePasswordVisibility = () => {
  showPassword.value = !showPassword.value;
};

// 切换确认密码可见性
const toggleConfirmPasswordVisibility = () => {
  showConfirmPassword.value = !showConfirmPassword.value;
};

const param = ref({
  userName: username.value,
  password: password.value,
  nickName: nickname.value,
  avatar: "avatar.png",
});

// ==================== 多色萤火虫 + 鼠标吹散（与登录页一致） ====================
const bgCanvas = ref(null);
const ctx = ref(null);
const fireflies = ref([]);
let animationId = null;
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
      radius: 2 + Math.random() * 3.5,
      speed: 0.25 + Math.random() * 0.55,
      direction: Math.random() * Math.PI * 2,
      baseAlpha: 0.55 + Math.random() * 0.35,
      flickerSpeed: 0.0011 + Math.random() * 0.0012,
      offset: Math.random() * Math.PI * 2,
      palette: Math.floor(Math.random() * 5), // 0 黄绿、1 青绿、2 浅橙
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
    // 漂浮
    f.direction += (Math.random() - 0.5) * 0.03;
    f.x += Math.cos(f.direction) * f.speed;
    f.y += Math.sin(f.direction) * f.speed * 0.7;

    // 越界循环
    if (f.x < -20) f.x = width + 20;
    if (f.x > width + 20) f.x = -20;
    if (f.y < -20) f.y = height + 20;
    if (f.y > height + 20) f.y = -20;

    // 鼠标吹散
    if (mouseX !== null && mouseY !== null) {
      const dx = f.x - mouseX;
      const dy = f.y - mouseY;
      const dist2 = dx * dx + dy * dy;
      const blowRadius = 220;
      const blowRadius2 = blowRadius * blowRadius;

      if (dist2 < blowRadius2) {
        const dist = Math.sqrt(dist2) || 1;
        const force = (blowRadius - dist) / blowRadius;
        const push = force * 14;
        f.x += (dx / dist) * push;
        f.y += (dy / dist) * push - force * 10;
      }
    }

    // 闪烁
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
  createFireflies(120);
};

// 鼠标事件
const handleMouseMove = (e) => {
  const canvas = bgCanvas.value;
  if (!canvas) return;
  const rect = canvas.getBoundingClientRect();
  mouseX = e.clientX - rect.left;
  mouseY = e.clientY - rect.top;
};

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
  ctx.value = canvas.getContext("2d");
  resizeCanvas();
  startAnimation();
};

onMounted(() => {
  initCanvas();
  window.addEventListener("resize", resizeCanvas);
  window.addEventListener("mousemove", handleMouseMove);
  window.addEventListener("mouseleave", handleMouseLeave);
});

onBeforeUnmount(() => {
  window.removeEventListener("resize", resizeCanvas);
  window.removeEventListener("mousemove", handleMouseMove);
  window.removeEventListener("mouseleave", handleMouseLeave);
  if (animationId) cancelAnimationFrame(animationId);
});

// 处理注册
const handleRegister = async () => {
  if (!isFormValid.value) {
    ElMessage.error("请检查表单填写是否正确");
    return;
  }

  try {
    param.value = {
      userName: username.value,
      password: password.value,
      nickName: nickname.value,
      avatar: "avatar.png",
    };
    const response = await apiClient.post(`user/add`, param.value, {
      headers: {
        "Content-Type": "application/json",
        Authorization: window.localStorage.token,
      },
    });
    if (response.flag == true) {
      ElNotification.success({
        title: "注册成功",
        message: "即将跳转到登录页面...",
        duration: 3000,
      });

      setTimeout(() => {
        router.push("/login");
      }, 3000);
    } else {
      ElMessage.error(response.data);
    }
  } catch (error) {
    console.error("请求失败", error);
    throw error;
  }
};
</script>
