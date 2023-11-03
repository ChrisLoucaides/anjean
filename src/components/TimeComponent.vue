<template>
  <div>
    <div class="time-container">
      <h2 class = "current-time"> <strong>{{ currentTime }}</strong></h2>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentTime: this.getCurrentTime()
    };
  },
  mounted() {
    setInterval(() => {
      this.currentTime = this.getCurrentTime();
    }, 1000);
  },
  methods: {
    getCurrentTime() {
      const now = new Date();
      const hours = now.getHours().toString().padStart(2, '0');
      const minutes = now.getMinutes().toString().padStart(2, '0');
      const seconds = now.getSeconds().toString().padStart(2, '0');
      return `${hours}:${minutes}:${seconds}`;
    }
  }
};
</script>

<style>
.time-container {
  width: 100px;
  text-align: center;
  margin: 0 auto;
}

.current-time {
  color: #19b6b1;
}
</style>
