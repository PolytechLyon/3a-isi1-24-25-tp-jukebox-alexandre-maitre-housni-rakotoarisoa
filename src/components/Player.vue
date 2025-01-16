<template>
  <div class="player">
    <h2>Player</h2>
    <p v-if="currentTrack">Now playing: {{ currentTrack.title }}</p>
    <p v-else>No track selected</p>

    <!-- Mode de répétition -->
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

    <!-- Contrôles du lecteur -->
    <div v-if="currentTrack">
      <button @click="togglePlay">{{ isPlaying ? 'Pause' : 'Play' }}</button>
      <button @click="playNext">Next</button>
      <div class="progress-bar" @click="seek">
        <div class="progress" :style="{ width: progress + '%' }"></div>
      </div>
      <audio
        ref="audio"
        :src="currentTrack.url"
        @timeupdate="updateProgress"
        @ended="onTrackEnd"
      ></audio>
    </div>

    <!-- Liste des pistes -->
    <h3>Playlist</h3>
    <ul>
      <li v-for="(track, index) in tracks" :key="index">
        <span>{{ track.title }}</span>
        <button @click="playTrack(index)">Play</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    tracks: {
      type: Array,
      required: true, // Liste des pistes
    },
  },
  data() {
    return {
      currentTrackIndex: -1, // Index de la piste en cours de lecture
      isPlaying: false, // Statut de lecture
      progress: 0, // Progression de la lecture
      repeatMode: 'list', // Modes : list, track, none
    };
  },
  computed: {
    currentTrack() {
      return this.tracks[this.currentTrackIndex] || null;
    },
  },
  methods: {
    playTrack(index) {
      this.currentTrackIndex = index; // Met à jour la piste en cours
      this.isPlaying = false; // Réinitialise le statut de lecture
      this.progress = 0; // Réinitialise la barre de progression
      this.$nextTick(() => {
        const audio = this.$refs.audio;
        if (audio) {
          audio.load(); // Recharge la piste
          audio.play(); // Joue la piste
          this.isPlaying = true;
        }
      });
    },
    togglePlay() {
      const audio = this.$refs.audio;
      if (!audio) return;

      if (this.isPlaying) {
        audio.pause();
      } else {
        audio.play();
      }
      this.isPlaying = !this.isPlaying;
    },
    playNext() {
      if (this.repeatMode === 'list') {
        const nextIndex = (this.currentTrackIndex + 1) % this.tracks.length;
        this.playTrack(nextIndex);
      } else if (this.repeatMode === 'none') {
        this.isPlaying = false;
      }
    },
    updateProgress(event) {
      const audio = event.target;
      if (audio.duration) {
        this.progress = (audio.currentTime / audio.duration) * 100;
      }
    },
    seek(event) {
      const audio = this.$refs.audio;
      const rect = event.target.getBoundingClientRect();
      const position = (event.clientX - rect.left) / rect.width;
      audio.currentTime = position * audio.duration;
    },
    onTrackEnd() {
      if (this.repeatMode === 'track') {
        // Rejouer la piste actuelle
        const audio = this.$refs.audio;
        audio.currentTime = 0;
        audio.play();
      } else if (this.repeatMode === 'list') {
        this.playNext();
      } else if (this.repeatMode === 'none') {
        this.isPlaying = false;
      }
    },
  },
};
</script>

<style scoped>
.player {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.progress-bar {
  width: 100%;
  height: 10px;
  background: #ccc;
  cursor: pointer;
  position: relative;
  margin: 10px 0;
}

.progress {
  height: 100%;
  background: #007bff;
}
</style>
