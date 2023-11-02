<template>
  <div>
    <h1>Current Time</h1>
    <p>{{ currentTime }}</p>
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
