<template>
  <div class="container" :class="{ 'birthday': isBirthday }">
    <div class="header">
      <h1>{{ title }}</h1>
    </div>
    
    <div class="content">
      <div class="love-days">
        <p>我们已经在一起</p>
        <div class="days-count">{{ daysTogether }}</div>
        <p>天了</p>
      </div>
      
      <div class="message" v-if="message">
        <div class="heart-icon">❤️</div>
        <p>{{ message }}</p>
      </div>
      
      <div class="next-special-day">
        <p>{{ nextSpecialDay }}</p>
      </div>
    </div>
    
    <div class="footer">
      <p>爱你的每一天 ❤️</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';

// 在这里设置你们在一起的日期（格式：年, 月-1, 日）
// 注意：月份是从0开始的，所以1月是0，2月是1，以此类推
const anniversaryDate = new Date(2023, 7, 13); // 示例日期，请修改为你们在一起的实际日期
const myBirthday = { month: 3, day: 15 };      // 4月15日 (月份从0开始)
const partnerBirthday = { month: 7, day: 13 }; // 8月13日 (月份从0开始)

const title = ref('给最爱的你');
const message = ref('');
const isBirthday = ref(false);

// 计算在一起的天数
const daysTogether = computed(() => {
  const today = new Date();
  const timeDiff = today - anniversaryDate;
  return Math.floor(timeDiff / (1000 * 60 * 60 * 24));
});

// 计算下一个特殊日期
const nextSpecialDay = computed(() => {
  const today = new Date();
  const currentYear = today.getFullYear();
  
  // 计算今年的两个生日日期
  const myBirthdayThisYear = new Date(currentYear, myBirthday.month, myBirthday.day);
  const partnerBirthdayThisYear = new Date(currentYear, partnerBirthday.month, partnerBirthday.day);
  
  // 如果生日已经过了，计算明年的日期
  const myBirthdayNextYear = new Date(currentYear + 1, myBirthday.month, myBirthday.day);
  const partnerBirthdayNextYear = new Date(currentYear + 1, partnerBirthday.month, partnerBirthday.day);
  
  // 计算距离下一个生日的天数
  let daysToMyBirthday, daysToPartnerBirthday;
  
  if (today <= myBirthdayThisYear) {
    daysToMyBirthday = Math.ceil((myBirthdayThisYear - today) / (1000 * 60 * 60 * 24));
  } else {
    daysToMyBirthday = Math.ceil((myBirthdayNextYear - today) / (1000 * 60 * 60 * 24));
  }
  
  if (today <= partnerBirthdayThisYear) {
    daysToPartnerBirthday = Math.ceil((partnerBirthdayThisYear - today) / (1000 * 60 * 60 * 24));
  } else {
    daysToPartnerBirthday = Math.ceil((partnerBirthdayNextYear - today) / (1000 * 60 * 60 * 24));
  }
  
  // 返回最近的生日信息
  if (daysToMyBirthday < daysToPartnerBirthday) {
    return `距离我的生日还有 ${daysToMyBirthday} 天`;
  } else {
    return `距离你的生日还有 ${daysToPartnerBirthday} 天`;
  }
});

// 检查是否是特殊日子
function checkSpecialDay() {
  const today = new Date();
  const currentMonth = today.getMonth();
  const currentDay = today.getDate();
  
  // 检查是否是我的生日
  if (currentMonth === myBirthday.month && currentDay === myBirthday.day) {
    message.value = '今天是我的生日！谢谢你陪我度过这特别的一天 ❤️';
    isBirthday.value = true;
  } 
  // 检查是否是对象的生日
  else if (currentMonth === partnerBirthday.month && currentDay === partnerBirthday.day) {
    message.value = '今天是你的生日！祝你生日快乐，我的爱 ❤️';
    isBirthday.value = true;
  } 
  // 普通日子的消息
  else {
    isBirthday.value = false;
    
    // 随机选择一条甜蜜的消息
    const sweetMessages = [
      '每一天有你的陪伴都是最好的礼物',
      '感谢你让我的生活变得如此美好',
      '和你在一起的每一刻都值得珍藏',
      '你是我最美的意外，最甜的惊喜',
      '希望未来的每一天都有你相伴',
      '你的笑容是我最大的幸福',
      '爱你是我做过最正确的决定'
    ];
    
    const randomIndex = Math.floor(Math.random() * sweetMessages.length);
    message.value = sweetMessages[randomIndex];
  }
}

onMounted(() => {
  // 页面加载时检查是否是特殊日子
  checkSpecialDay();
  
  // 每天凌晨更新一次（处理日期变化）
  const now = new Date();
  const tomorrow = new Date(now.getFullYear(), now.getMonth(), now.getDate() + 1);
  const timeToMidnight = tomorrow - now;
  
  setTimeout(() => {
    checkSpecialDay();
    // 之后每24小时检查一次
    setInterval(checkSpecialDay, 24 * 60 * 60 * 1000);
  }, timeToMidnight);
});
</script>

<style scoped>
.container {
  background-color: #fff;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 500px;
  overflow: hidden;
  transition: all 0.5s ease;
  margin: 0 auto;
}

.container.birthday {
  background: linear-gradient(135deg, #ff9a9e 0%, #fad0c4 100%);
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba(255, 105, 180, 0.7);
  }
  70% {
    box-shadow: 0 0 0 20px rgba(255, 105, 180, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(255, 105, 180, 0);
  }
}

.header {
  background-color: #ff6b6b;
  color: white;
  padding: 20px;
  text-align: center;
}

.birthday .header {
  background-color: #ff4081;
}

.content {
  padding: 30px;
  text-align: center;
}

.love-days {
  margin-bottom: 30px;
}

.days-count {
  font-size: 4rem;
  font-weight: bold;
  color: #ff6b6b;
  margin: 10px 0;
  font-family: 'Ma Shan Zheng', cursive;
}

.birthday .days-count {
  color: #ff4081;
}

.message {
  background-color: rgba(255, 107, 107, 0.1);
  border-radius: 10px;
  padding: 20px;
  margin: 20px 0;
  position: relative;
}

.birthday .message {
  background-color: rgba(255, 64, 129, 0.2);
}

.heart-icon {
  font-size: 2rem;
  margin-bottom: 10px;
  animation: heartbeat 1.5s infinite;
}

@keyframes heartbeat {
  0% {
    transform: scale(1);
  }
  25% {
    transform: scale(1.1);
  }
  50% {
    transform: scale(1);
  }
  75% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}

.next-special-day {
  margin-top: 20px;
  font-style: italic;
  color: #666;
}

.footer {
  background-color: #f8f9fa;
  padding: 15px;
  text-align: center;
  font-size: 0.9rem;
  color: #666;
}

@media (max-width: 600px) {
  .container {
    border-radius: 0;
  }
  
  .days-count {
    font-size: 3rem;
  }
}
</style>