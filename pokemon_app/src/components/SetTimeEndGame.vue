<template>
  <div class="time_to_end">Time: {{ countdown }} seconds</div>
</template>
<script>
export default {
  data() {
    return {
      countdown: 60,
      timer: null,
    };
  },
  mounted() {
    this.startCountdown();
  },
  methods: {
    startCountdown() {
      this.timer = setInterval(() => {
        if (this.countdown > 0) {
          this.countdown--;
        } else {
          this.stopCountdown();
        }
      }, 1000);
    },
    stopCountdown() {
      clearInterval(this.timer);
    },
  },
  beforeUnmount() {
    this.stopCountdown();
  },
};
</script>
<style lang="css" scoped>
.time_to_end {
  position: fixed;
  top: 0;
  right: 0;
  margin: 2rem;
  font-size: 2rem;
}
</style>
