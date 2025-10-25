<template>
  <div class="banner-container" @mouseenter="stopAutoPlay" @mouseleave="startAutoPlay">
    <!-- 轮播容器：通过transform控制滑动 -->
    <div 
      class="banner-wrapper" 
      :style="{ transform: `translateX(-${currentIndex * 100}%)` }"
    >
      <!-- 循环渲染3组轮播项（每组5张图） -->
      <div class="banner-slide" v-for="(group, groupIndex) in imageGroups" :key="groupIndex">
        <img 
          v-for="(img, imgIndex) in group" 
          :key="imgIndex" 
          :src="img.url" 
          :alt="`第${groupIndex+1}组-${imgIndex+1}`"
          class="slide-img"
        >
      </div>
    </div>

    <!-- 上/下一组按钮 -->
    <button class="control-btn prev-btn" @click="prevGroup">&lt;</button>
    <button class="control-btn next-btn" @click="nextGroup">&gt;</button>

    <!-- 轮播指示器 -->
    <div class="indicators">
      <div 
        class="indicator-dot" 
        v-for="(_, index) in imageGroups.length" 
        :key="index"
        :class="{ active: currentIndex === index }"
        @click="switchToGroup(index)"
      ></div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';

// 1. 原始图片数据（15张图的URL）
const images = [
  { url: 'https://i.ibb.co/1fsSp9g8/1.png' },
  { url: 'https://i.ibb.co/1fZNkWdP/2.png' },
  { url: 'https://i.ibb.co/sJjkjNJj/3.png' },
  { url: 'https://i.ibb.co/5gQL5f8F/4.png' },
  { url: 'https://i.ibb.co/8D6YtVX7/5.png' },
  { url: 'https://i.ibb.co/JWvDkrZn/6.png' },
  { url: 'https://i.ibb.co/zVyj8kFP/7.png' },
  { url: 'https://i.ibb.co/7Nyn6D4W/8.png' },
  { url: 'https://i.ibb.co/F46byhWm/9.png' },
  { url: 'https://i.ibb.co/zTZ4qkBY/10.png' },
  { url: 'https://i.ibb.co/7m21bDH/11.png' },
  { url: 'https://i.ibb.co/7NQ656v8/12.png' },
  { url: 'https://i.ibb.co/wFqxTsmC/13.png' },
  { url: 'https://i.ibb.co/0j2pXcNT/14.png' },
  { url: 'https://i.ibb.co/3m13cqPp/15.png' }
];

// 2. 按每5张一组拆分图片（15张 → 3组）
const imageGroups = ref([]);
// 分组逻辑：每5个元素切分一次
for (let i = 0; i < images.length; i += 5) {
  imageGroups.value.push(images.slice(i, i + 5));
}

// 3. 轮播状态管理
const currentIndex = ref(0); // 当前显示的组索引（0-2）
let autoPlayTimer = null; // 自动轮播定时器

// 4. 轮播控制方法
// 切换到指定组
const switchToGroup = (index) => {
  currentIndex.value = index;
};

// 上一组
const prevGroup = () => {
  currentIndex.value = currentIndex.value === 0 
    ? imageGroups.value.length - 1 
    : currentIndex.value - 1;
};

// 下一组
const nextGroup = () => {
  currentIndex.value = currentIndex.value === imageGroups.value.length - 1 
    ? 0 
    : currentIndex.value + 1;
};

// 5. 自动轮播功能
const startAutoPlay = () => {
  // 清除已有定时器，避免重复触发
  if (autoPlayTimer) clearInterval(autoPlayTimer);
  // 每5秒切换一次
  autoPlayTimer = setInterval(() => {
    nextGroup();
  }, 5000);
};

const stopAutoPlay = () => {
  if (autoPlayTimer) clearInterval(autoPlayTimer);
};

// 6. 生命周期管理
onMounted(() => {
  // 组件挂载后启动自动轮播
  startAutoPlay();
});

onBeforeUnmount(() => {
  // 组件销毁前清除定时器，防止内存泄漏
  stopAutoPlay();
});
</script>

<style scoped>
.banner-container {
  width: 1000px; /* 轮播图总宽度（可自定义） */
  height: 220px; /* 轮播图高度（可自定义） */
  margin: 0 auto;
  position: relative;
  overflow: hidden; /* 隐藏超出容器的内容 */
  border-radius: 8px; /* 可选：圆角效果 */
  box-shadow: 0 2px 8px rgba(0,0,0,0.1); /* 可选：阴影效果 */
}

/* 轮播容器：横向排列所有组 */
.banner-wrapper {
  display: flex;
  width: 100%;
  height: 100%;
  transition: transform 0.5s ease-in-out; /* 滑动动画 */
}

/* 单个轮播组（包含5张图） */
.banner-slide {
  flex: 1 0 100%; /* 每个组占满容器宽度 */
  display: flex;
  height: 100%;
}

/* 组内单张图片样式 */
.slide-img {
  flex: 1; /* 5张图平均分配宽度 */
  height: 100%;
  object-fit: cover; /* 图片裁剪适配，保持比例 */
  border-right: 1px solid #fff; /* 图片间分隔线（可选） */
}
/* 去掉最后一张图的右边框 */
.slide-img:last-child {
  border-right: none;
}

/* 控制按钮样式 */
.control-btn {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 40px;
  height: 40px;
  background: rgba(0,0,0,0.3);
  color: white;
  border: none;
  border-radius: 50%;
  font-size: 18px;
  cursor: pointer;
  z-index: 10;
  transition: background 0.3s;
}
.control-btn:hover {
  background: rgba(0,0,0,0.5);
}
.prev-btn { left: 15px; }
.next-btn { right: 15px; }

/* 指示器样式 */
.indicators {
  position: absolute;
  bottom: 15px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 8px;
}

.indicator-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: rgba(255,255,255,0.5);
  cursor: pointer;
  transition: background 0.3s;
}
.indicator-dot.active {
  background: white; /* 当前组指示器高亮 */
  width: 24px; /* 激活状态宽度拉长（可选） */
  border-radius: 5px;
}
</style>