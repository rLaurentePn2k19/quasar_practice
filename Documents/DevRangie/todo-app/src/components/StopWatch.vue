<template>
  <div class="q-pa-md row items-start q-gutter-md">
    <q-card class="my-card bg-secondary text-white">
      <q-card-section>
        <div class="text-h6">Stop Watch</div>
        <!-- <div class="text-subtitle2">by John Doe</div> -->
      </q-card-section>
      <q-separator dark/>
      
      <q-card-section class="text-h2">
        <div id="timer">
          <span id="minutes">{{ minutes }}</span>
          <span id="middle">:</span>
          <span id="seconds">{{ seconds }}</span>
        </div>
      </q-card-section>

      <q-separator dark/>

      <q-card-actions>
        <q-btn flat v-if="!timer" @click="startTimer">Start</q-btn>
        <q-btn flat v-if="timer" @click="stopTimer">Pause</q-btn>
        <q-btn flat v-if="resetButton" @click="resetTimer">Reset</q-btn>
      </q-card-actions>
    </q-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      lorem: "test",
      timer: null,
      totalTime: 25 * 60,
      resetButton: false
      //   title: "Let the countdown begin!!"
    };
  },
  methods: {
    startTimer: function() {
      this.timer = setInterval(() => this.countdown(), 1000);
      this.resetButton = true;
      //   this.title = "Greatness is within sight!!";
    },
    stopTimer: function() {
      clearInterval(this.timer);
      this.timer = null;
      this.resetButton = true;
      //   this.title = "Never quit, keep going!!";
    },
    resetTimer: function() {
      this.totalTime = 25 * 60;
      clearInterval(this.timer);
      this.timer = null;
      this.resetButton = false;
      //   this.title = "Let the countdown begin!!";
    },
    padTime: function(time) {
      return (time < 10 ? "0" : "") + time;
    },
    countdown: function() {
      if (this.totalTime >= 1) {
        this.totalTime--;
      } else {
        this.totalTime = 0;
        this.resetTimer();
      }
    }
  },
  // ========================
  computed: {
    minutes: function() {
      const minutes = Math.floor(this.totalTime / 60);
      return this.padTime(minutes);
    },
    seconds: function() {
      const seconds = this.totalTime - this.minutes * 60;
      return this.padTime(seconds);
    }
  }
};
</script>

<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 250px
</style>