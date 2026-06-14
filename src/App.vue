<script setup>
import { ref, onMounted, watch } from 'vue'
import Counter from './components/Counter.vue'

// 响应式数据
const isDark = ref(false)
const currentTime = ref(new Date())
const greeting = ref('')

// 根据时间设置问候语
const setGreeting = () => {
  const hour = new Date().getHours()
  if (hour < 6) greeting.value = '夜深了'
  else if (hour < 12) greeting.value = '早上好'
  else if (hour < 18) greeting.value = '下午好'
  else greeting.value = '晚上好'
}

// 主题切换
const toggleTheme = () => {
  isDark.value = !isDark.value
}

// 监听主题变化，持久化到localStorage
watch(isDark, (newValue) => {
  localStorage.setItem('theme', newValue ? 'dark' : 'light')
  document.body.classList.toggle('dark-theme', newValue)
})

// 组件挂载时初始化
onMounted(() => {
  // 初始化主题
  const savedTheme = localStorage.getItem('theme')
  isDark.value = savedTheme === 'dark'
  document.body.classList.toggle('dark-theme', isDark.value)
  
  // 初始化问候语
  setGreeting()
  
  // 更新时间
  setInterval(() => {
    currentTime.value = new Date()
  }, 1000)

  // 页面加载动画
  document.body.style.opacity = '0'
  setTimeout(() => {
    document.body.style.transition = 'opacity 0.5s'
    document.body.style.opacity = '1'
  }, 100)
})
</script>

<template>
  <div class="app" :class="{ 'dark-app': isDark }">
    <!-- 导航栏 -->
    <nav class="navbar">
      <div class="nav-container">
        <div class="logo">
          <span class="logo-icon">✨</span>
          <span class="logo-text">Awesome Web Starter</span>
        </div>
        <div class="nav-links">
          <a href="#features" class="nav-link">特性</a>
          <a href="#demo" class="nav-link">演示</a>
          <a href="#quickstart" class="nav-link">快速开始</a>
          <button class="theme-btn" @click="toggleTheme">
            {{ isDark ? '☀️' : '🌙' }}
          </button>
        </div>
      </div>
    </nav>

    <!-- Hero区域 -->
    <section class="hero">
      <div class="hero-content">
        <div class="greeting">{{ greeting }}，欢迎使用 👋</div>
        <h1 class="hero-title">
          <span class="gradient-text">Awesome Web Starter</span>
        </h1>
        <p class="hero-subtitle">
          一个零依赖、开箱即用、现代化的纯前端网页启动模板<br>
          助你快速构建精美网站
        </p>
        <div class="hero-cta">
          <a href="https://github.com/1ukvcn/awesome-web-starter" target="_blank" class="btn btn-primary">
            ⭐ GitHub Star
          </a>
          <a href="#quickstart" class="btn btn-secondary">
            🚀 快速开始
          </a>
        </div>
        <div class="current-time">
          🕐 {{ currentTime.toLocaleTimeString('zh-CN') }}
        </div>
      </div>
    </section>

    <!-- 特性区域 -->
    <section id="features" class="features">
      <h2 class="section-title">✨ 核心特性</h2>
      <div class="features-grid">
        <div class="feature-card">
          <div class="feature-icon">⚡</div>
          <h3>3秒启动</h3>
          <p>零配置开箱即用，无需复杂的构建工具和环境搭建</p>
        </div>
        <div class="feature-card">
          <div class="feature-icon">🎨</div>
          <h3>现代化设计</h3>
          <p>毛玻璃效果+渐变风格，支持深浅主题切换</p>
        </div>
        <div class="feature-card">
          <div class="feature-icon">📱</div>
          <h3>完美响应式</h3>
          <p>全设备自适应，移动端完美展示</p>
        </div>
        <div class="feature-card">
          <div class="feature-icon">🔧</div>
          <h3>极致可定制</h3>
          <p>CSS变量统一管理，轻松修改配色和样式</p>
        </div>
      </div>
    </section>

    <!-- 演示区域 -->
    <section id="demo" class="demo">
      <h2 class="section-title">🎮 交互演示</h2>
      <div class="demo-container">
        <Counter />
      </div>
    </section>

    <!-- 快速开始 -->
    <section id="quickstart" class="quickstart">
      <h2 class="section-title">🚀 快速开始</h2>
      <div class="code-block">
        <pre><code># 安装依赖
npm install

# 启动开发服务器
npm run dev

# 构建生产版本
npm run build</code></pre>
      </div>
    </section>

    <!-- Footer -->
    <footer class="footer">
      <p>Made with ❤️ by Awesome Web Starter</p>
      <p class="footer-note">
        本项目完全由豆包（办公模式）生成，项目作者仅完成GitHub账号登录操作
      </p>
    </footer>

    <!-- 返回顶部按钮 -->
    <button class="back-to-top" @click="window.scrollTo({ top: 0, behavior: 'smooth' })">
      ↑
    </button>
  </div>
</template>

<style scoped>
.app {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  transition: all 0.3s ease;
}

.dark-app {
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
}

/* 导航栏 */
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  backdrop-filter: blur(20px);
  background: rgba(255, 255, 255, 0.1);
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
}

.nav-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 1rem 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: white;
  font-weight: bold;
  font-size: 1.2rem;
}

.nav-links {
  display: flex;
  gap: 2rem;
  align-items: center;
}

.nav-link {
  color: white;
  text-decoration: none;
  opacity: 0.9;
  transition: opacity 0.3s;
}

.nav-link:hover {
  opacity: 1;
}

.theme-btn {
  background: rgba(255, 255, 255, 0.2);
  border: none;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  cursor: pointer;
  font-size: 1.2rem;
  transition: transform 0.3s;
}

.theme-btn:hover {
  transform: scale(1.1);
}

/* Hero区域 */
.hero {
  padding: 8rem 2rem 4rem;
  text-align: center;
  color: white;
}

.greeting {
  font-size: 1.2rem;
  opacity: 0.9;
  margin-bottom: 1rem;
}

.hero-title {
  font-size: clamp(2rem, 5vw, 4rem);
  margin-bottom: 1rem;
}

.gradient-text {
  background: linear-gradient(90deg, #fff, #f0f0ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-subtitle {
  font-size: 1.2rem;
  opacity: 0.9;
  line-height: 1.8;
  margin-bottom: 2rem;
}

.hero-cta {
  display: flex;
  gap: 1rem;
  justify-content: center;
  flex-wrap: wrap;
  margin-bottom: 2rem;
}

.btn {
  padding: 0.8rem 2rem;
  border-radius: 50px;
  text-decoration: none;
  font-weight: 600;
  transition: all 0.3s;
}

.btn-primary {
  background: white;
  color: #667eea;
}

.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
}

.btn-secondary {
  background: rgba(255, 255, 255, 0.2);
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.3);
}

.btn-secondary:hover {
  background: rgba(255, 255, 255, 0.3);
}

.current-time {
  font-size: 1rem;
  opacity: 0.8;
}

/* 通用区域样式 */
section {
  padding: 4rem 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.section-title {
  text-align: center;
  font-size: 2rem;
  color: white;
  margin-bottom: 3rem;
}

/* 特性卡片 */
.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
}

.feature-card {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 2rem;
  text-align: center;
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.2);
  transition: transform 0.3s;
}

.feature-card:hover {
  transform: translateY(-5px);
}

.feature-icon {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.feature-card h3 {
  margin-bottom: 1rem;
  font-size: 1.3rem;
}

.feature-card p {
  opacity: 0.9;
  line-height: 1.6;
}

/* 演示区域 */
.demo-container {
  display: flex;
  justify-content: center;
}

/* 代码块 */
.code-block {
  background: rgba(0, 0, 0, 0.3);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 2rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.code-block pre {
  margin: 0;
  color: #a5d6ff;
  font-family: 'Fira Code', monospace;
  line-height: 1.8;
}

/* Footer */
.footer {
  text-align: center;
  padding: 3rem 2rem;
  color: white;
  opacity: 0.8;
}

.footer-note {
  margin-top: 1rem;
  font-size: 0.9rem;
  opacity: 0.7;
}

/* 返回顶部 */
.back-to-top {
  position: fixed;
  bottom: 2rem;
  right: 2rem;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  color: white;
  font-size: 1.5rem;
  cursor: pointer;
  transition: all 0.3s;
}

.back-to-top:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: translateY(-3px);
}

/* 响应式 */
@media (max-width: 768px) {
  .nav-links {
    gap: 1rem;
  }
  
  .nav-link {
    display: none;
  }
  
  .hero-cta {
    flex-direction: column;
    align-items: center;
  }
}
</style>
