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
      <button @click="playNext" :disabled="tracks.length === 0">Next</button>
      <div class="progress-bar" @click="seek">
        <div class="progress" :style="{ width: progress + '%' }"></div>
      </div>
      <audio
        ref="audio"
        :src="currentTrack.url"
        @timeupdate="updateProgress"
        @ended="onTrackEnd"
        @error="handleAudioError"
      ></audio>
    </div>

    <!-- Liste des pistes -->
    <h3>Playlist</h3>
    <ul>
      <li v-for="(track, index) in tracks" :key="index">
        <span :class="{ broken: track.broken }">{{ track.title }}</span>
        <button @click="playTrack(index)" :disabled="track.broken">Play</button>
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
  const track = this.tracks[index];

  // Vérifier si la piste est déjà marquée comme invalide
  if (track.broken) {
    console.warn(`Track "${track.title}" is marked as broken. Skipping playback.`);
    this.playNext(); // Passer à la piste suivante automatiquement
    return;
  }

  // Mettre à jour la piste en cours
  this.currentTrackIndex = index;
  this.isPlaying = false; // Réinitialise le statut de lecture
  this.progress = 0; // Réinitialise la barre de progression

  this.$nextTick(() => {
    const audio = this.$refs.audio;
    if (audio) {
      // Essayer de charger et de jouer la piste
      audio.load();
      audio
        .play()
        .then(() => {
          this.isPlaying = true; // Lecture réussie
        })
        .catch((error) => {
          console.error(`Failed to play track "${track.title}".`, error);
          this.$set(this.tracks, index, { ...track, broken: true }); // Marquer uniquement cette piste
          this.playNext(); // Passer à la piste suivante automatiquement
        });
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
  const totalTracks = this.tracks.length;
  if (totalTracks === 0) return; // Pas de pistes disponibles

  let nextIndex = this.currentTrackIndex;

  do {
    nextIndex = (nextIndex + 1) % totalTracks; // Avancer à la piste suivante (en boucle)
    
    // Si on a fait un tour complet et qu'il n'y a pas de pistes valides
    if (nextIndex === this.currentTrackIndex && this.tracks[nextIndex].broken) {
      console.warn('No valid tracks to play.');
      this.isPlaying = false;
      return;
    }
  } while (this.tracks[nextIndex].broken); // Sauter les pistes marquées comme "broken"

  this.playTrack(nextIndex); // Jouer la prochaine piste valide
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
    this.playNext(); // Appelle la nouvelle logique de "playNext"
  } else if (this.repeatMode === 'none') {
    this.isPlaying = false;
  }
},

    handleAudioError() {
      console.error('Audio file is invalid or cannot be played. Skipping to the next track.');
      this.tracks[this.currentTrackIndex].broken = true; // Marquer la piste comme invalide
      if (this.repeatMode === 'list' || this.repeatMode === 'none') {
        this.playNext(); // Passer à la piste suivante
      } else if (this.repeatMode === 'track') {
        this.onTrackEnd(); // Rejouer ou arrêter
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

.broken {
  text-decoration: line-through;
  color: red;
}
</style>
