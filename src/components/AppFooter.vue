<template>
  <footer class="app-footer" :class="{ 'expanded': isExpanded }">
    <!-- 压缩状态 -->
    <div v-if="!isExpanded" class="footer-minimal" @click="toggleFooter">
      <div class="footer-toggle">
        <div class="footer-glow"></div>
        <span class="footer-text">© {{ currentYear }} AI-SIAT - 让AI为你谱写浪漫 💖</span>
        <span class="footer-arrow">{{ isExpanded ? '▲' : '▼' }}</span>
      </div>
    </div>

    <!-- 展开状态 -->
    <div v-if="isExpanded" class="footer-expanded">
      <div class="footer-toggle" @click="toggleFooter">
        <span class="footer-arrow">{{ isExpanded ? '▲' : '▼' }}</span>
      </div>

      <div class="footer-content">
        <div class="footer-section">
          <div class="footer-logo">
            <h3>AI LOVE 智能体应用平台</h3>
          </div>
          <div class="footer-links">
            <a href="#">《用户协议》</a>
            <a href="#">《隐私政策》</a>
          </div>
        </div>

        <div class="footer-section">
          <h4>友情链接</h4>
          <div class="footer-links">
            <a href="http://yunikon-picture.user-center-yzj.top/" target="_blank">Yunikon云端智能图库</a>
          </div>
        </div>

        <div class="footer-section">
          <h4>联系我们</h4>
          <div class="footer-links">
            <a href="#">商务合作</a>
            <a href="https://blog.csdn.net/2301_79732484" target="_blank">站长：yunikon</a>
          </div>
        </div>
      </div>

      <div class="footer-bottom">
        <p>© {{ currentYear }} AI LOVE 智能体应用平台 - 让AI为你谱写浪漫 💖</p>
      </div>
    </div>
  </footer>
</template>

<script setup>
import { computed, ref, onMounted, onUnmounted } from 'vue'

// 计算当前年份
const currentYear = computed(() => new Date().getFullYear())

// 控制 footer 展开状态
const isExpanded = ref(false)

// 切换 footer 状态
const toggleFooter = () => {
  if (isExpanded.value) {
    // 收起时添加延迟，让动画更自然
    setTimeout(() => {
      isExpanded.value = false
    }, 150)
  } else {
    isExpanded.value = true
  }
}

// 监听滚动事件，自动展开 footer
let scrollTimeout = null
const handleScroll = () => {
  if (scrollTimeout) {
    clearTimeout(scrollTimeout)
  }

  scrollTimeout = setTimeout(() => {
    const scrollTop = window.pageYOffset || document.documentElement.scrollTop
    const windowHeight = window.innerHeight
    const documentHeight = document.documentElement.scrollHeight

    // 如果滚动到底部附近，自动展开 footer
    if (scrollTop + windowHeight >= documentHeight - 100) {
      isExpanded.value = true
    }
  }, 100)
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
  if (scrollTimeout) {
    clearTimeout(scrollTimeout)
  }
})
</script>

<style scoped>
.app-footer {
  background: linear-gradient(135deg, #ff006e, #ff4081);
  color: #666;
  width: 100%;
  margin-top: auto;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 -2px 20px rgba(255, 0, 110, 0.15);
  backdrop-filter: blur(10px);
}

.app-footer.expanded {
  height: auto;
  min-height: 200px;
  animation: expandUp 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

@keyframes expandUp {
  from {
    transform: translateY(100%);
    opacity: 0.8;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

.app-footer:not(.expanded) {
  height: 40px;
  cursor: pointer;
}

.footer-minimal {
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.footer-toggle {
  display: flex;
  align-items: center;
  gap: 10px;
  color: white;
  font-size: 14px;
  font-weight: 500;
  transition: all 0.3s ease;
  position: relative;
  padding: 5px 15px;
  border-radius: 20px;
}

.footer-toggle:hover {
  background: rgba(255, 255, 255, 0.1);
  transform: translateY(-2px);
}

.footer-glow {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  background: radial-gradient(circle, rgba(255, 255, 255, 0.3), transparent 70%);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: all 0.3s ease;
  pointer-events: none;
}

.footer-toggle:hover .footer-glow {
  width: 120px;
  height: 120px;
  opacity: 0.6;
}

.footer-text {
  text-shadow: 0 0 5px rgba(255, 255, 255, 0.3);
}

.footer-arrow {
  font-size: 12px;
  transition: transform 0.3s ease;
}

.app-footer.expanded .footer-arrow {
  transform: rotate(180deg);
}

.footer-expanded {
  padding: 20px 0;
  animation: fadeInUp 0.4s ease-out 0.1s both;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.footer-content {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  padding: 20px 20px 0;
  background: rgba(255, 255, 255, 0.95);
  margin-top: 20px;
  border-radius: 20px 20px 0 0;
  box-shadow: 0 -4px 20px rgba(0, 0, 0, 0.1);
  animation: contentSlideIn 0.5s ease-out 0.2s both;
}

@keyframes contentSlideIn {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.footer-section {
  flex: 1;
  min-width: 200px;
  margin-bottom: 30px;
  padding: 0 15px;
  animation: sectionFadeIn 0.4s ease-out both;
}

.footer-section:nth-child(1) {
  animation-delay: 0.3s;
}

.footer-section:nth-child(2) {
  animation-delay: 0.4s;
}

.footer-section:nth-child(3) {
  animation-delay: 0.5s;
}

@keyframes sectionFadeIn {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.footer-logo h3 {
  font-size: 1.25rem;
  margin-bottom: 15px;
  color: #333;
  font-weight: bold;
}

.footer-section h4 {
  font-size: 1rem;
  margin-bottom: 15px;
  color: #ff006e;
  font-weight: 600;
}

.footer-links {
  display: flex;
  flex-direction: column;
}

.footer-links a {
  margin-bottom: 10px;
  color: #666;
  text-decoration: none;
  transition: all 0.3s ease;
  padding: 5px 0;
  border-radius: 4px;
  position: relative;
}

.footer-links a:hover {
  color: #ff006e;
  transform: translateX(5px);
  text-shadow: 0 0 5px rgba(255, 0, 110, 0.3);
}

.qrcode {
  display: flex;
  align-items: center;
}

.qrcode-container {
  text-align: center;
}

.qrcode-placeholder {
  width: 90px;
  height: 90px;
  background-color: #f5f5f5;
  margin: 0 auto 10px;
  border-radius: 4px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
}

.qrcode-placeholder:after {
  content: '🤖';
}

.footer-bottom {
  text-align: center;
  padding-top: 20px;
  margin-top: 20px;
  border-top: 1px solid #eee;
  color: #999;
  font-size: 0.9rem;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 0 0 20px 20px;
  animation: bottomFadeIn 0.4s ease-out 0.6s both;
}

@keyframes bottomFadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* 响应式设计 */
@media (max-width: 768px) {
  .footer-content {
    flex-direction: column;
  }

  .footer-section {
    width: 100%;
    margin-bottom: 20px;
    padding: 0;
  }

  .footer-minimal {
    height: 35px;
  }

  .footer-toggle {
    font-size: 13px;
    padding: 3px 10px;
  }

  .app-footer:not(.expanded) {
    height: 35px;
  }

  .footer-toggle:hover .footer-glow {
    width: 100px;
    height: 100px;
  }
}

@media (max-width: 480px) {
  .footer-minimal {
    height: 30px;
  }

  .footer-toggle {
    font-size: 12px;
    padding: 2px 8px;
  }

  .app-footer:not(.expanded) {
    height: 30px;
  }

  .footer-section h4 {
    font-size: 0.95rem;
  }

  .footer-links a {
    font-size: 0.9rem;
  }

  .footer-toggle:hover .footer-glow {
    width: 80px;
    height: 80px;
  }
}
</style>
