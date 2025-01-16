<template>
  <div class="player">
    <h2>Player</h2>
    <p>Choose a track to play.</p>
    <fieldset>
      <legend>Playback mode</legend>
      <label>
        <input type="radio" value="list" v-model="repeatMode" /> Repeat list
      </label>
      <label>
        <input type="radio" value="track" v-model="repeatMode" /> Repeat track
      </label>
      <label>
        <input type="radio" value="none" v-model="repeatMode" /> Don't repeat
      </label>
    </fieldset>
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
