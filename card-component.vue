<template>
   <div class="card-list">
     <!-- 循环渲染15张卡片 -->
     <div class="card" v-for="(item, index) in cardList" :key="index">
       <img :src="item.imgUrl" :alt="'卡片图片' + (index+1)" class="card-img">
       <div class="card-content">
         <h3>{{ item.title }}</h3>
         <p>{{ item.desc }}</p>
       </div>
     </div>
   </div>
 </template>
 
 <script>
 export default {
   data() {
     return {
       // 卡片数据数组，存入图片URL和内容
       cardList: [
         { imgUrl: "https://i.ibb.co/1fsSp9g8/1.png?v=1", title: "可爱猫咪", desc: "温馨可爱的猫咪形象，传递温暖祝福" },
         { imgUrl: "https://i.ibb.co/1fZNkWdP/2.png?v=1", title: "邮票花卉", desc: "经典邮票设计，展现传统与优雅" },
         { imgUrl: "https://i.ibb.co/sJjkjNJj/3.png?v=1", title: "可爱兔子", desc: "萌系兔子形象，传递甜蜜祝福" },
         { imgUrl: "https://i.ibb.co/5gQL5f8F/4.png?v=1", title: "荔枝吉祥", desc: "荔枝图案设计，寓意吉祥如意" },
         { imgUrl: "https://i.ibb.co/8D6YtVX7/5.png?v=1", title: "红色福字", desc: "睡莲，简约大方，寓意吉祥如意" },
         { imgUrl: "https://i.ibb.co/JWvDkrZn/6.png?v=1", title: "金色福字", desc: "古风歌词设计，搭配背景，传统与现代结合" },
         { imgUrl: "https://i.ibb.co/zVyj8kFP/7.png?v=1", title: "可爱小鹿", desc: "卡通风格，可爱歌词形象，温馨可爱" },
         { imgUrl: "https://i.ibb.co/7Nyn6D4W/8.png?v=1", title: "金色福字", desc: "歌词设计，搭配红色背景，传统与现代结合" },
         { imgUrl: "https://i.ibb.co/F46byhWm/9.png?v=1", title: "金色福字", desc: "古风意境设计，搭配背景，传统与现代结合" },
         { imgUrl: "https://i.ibb.co/zTZ4qkBY/10.png?v=1", title: "金色福字", desc: "古风意境，搭配背景，传统与现代结合" },
         { imgUrl: "https://i.ibb.co/7m21bDH/11.png?v=1", title: "新封面11", desc: "精美设计，传递温馨祝福" },
         { imgUrl: "https://i.ibb.co/7NQ656v8/12.png?v=1", title: "新封面12", desc: "精美设计，传递温馨祝福" },
         { imgUrl: "https://i.ibb.co/wFqxTsmC/13.png?v=1", title: "新封面13", desc: "精美设计，传递温馨祝福" },
         { imgUrl: "https://i.ibb.co/0j2pXcNT/14.png?v=1", title: "新封面14", desc: "精美设计，传递温馨祝福" },
         { imgUrl: "https://i.ibb.co/3m13cqPp/15.png?v=1", title: "新封面15", desc: "精美设计，传递温馨祝福" }
       ]
     };
   }
 };
 </script>
 
 <style scoped>
 .card-list { display: flex; flex-wrap: wrap; gap: 20px; }
 .card { width: 200px; border: 1px solid #eee; border-radius: 8px; overflow: hidden; }
 .card-img { width: 100%; height: 150px; object-fit: cover; }
 .card-content { padding: 10px; }
 </style>