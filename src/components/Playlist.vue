<template>
  <div class="playlist">
    <h3>Playlist</h3>
    <p>Choose a track to play.</p>
    <ul>
      <li
        v-for="(track, index) in tracks"
        :key="index"
        :class="{ current: index === currentTrackIndex, broken: track.broken }"
      >
        <span>{{ track.title }}</span>
        <button @click="playTrack(index)" :disabled="track.broken">Play</button>
        <button @click="removeTrack(index)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    tracks: {
      type: Array,
      required: true,
    },
    currentTrackIndex: {
      type: Number,
      required: false,
      default: -1,
    },
  },
  methods: {
    playTrack(index) {
      if (!this.tracks[index].broken) {
        this.$emit('play-track', index);
      }
    },
    removeTrack(index) {
      this.$emit('remove-track', index);
    },
  },
};
</script>

<style scoped>
.current {
  font-weight: bold;
}

.broken {
  text-decoration: line-through;
}
</style>
