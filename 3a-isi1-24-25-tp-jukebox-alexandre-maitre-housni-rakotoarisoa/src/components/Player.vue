<template>
  <div class="player">
    <h2>Now Playing: {{ currentTrack?.title || 'None' }}</h2>
    <audio ref="audio" :src="currentTrack?.url" @timeupdate="updateProgress"></audio>
    <div>
      <button @click="togglePlay">{{ isPlaying ? 'Pause' : 'Play' }}</button>
      <span>Mode:</span>
      <select v-model="repeatMode">
        <option value="list">Repeat List</option>
        <option value="track">Repeat Track</option>
        <option value="none">No Repeat</option>
      </select>
    </div>
    <div class="progress-bar" @click="seek">
      <div :style="{ width: progress + '%' }"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentTrack: null,
      isPlaying: false,
      repeatMode: 'list',
      progress: 0,
    };
  },
  methods: {
    togglePlay() {
      const audio = this.$refs.audio;
      if (this.isPlaying) {
        audio.pause();
      } else {
        audio.play();
      }
      this.isPlaying = !this.isPlaying;
    },
    updateProgress(event) {
      const audio = event.target;
      this.progress = (audio.currentTime / audio.duration) * 100;
    },
    seek(event) {
      const audio = this.$refs.audio;
      const rect = event.target.getBoundingClientRect();
      const position = (event.clientX - rect.left) / rect.width;
      audio.currentTime = position * audio.duration;
    },
  },
};
</script>

<style scoped>
/* Style du lecteur */
</style>
