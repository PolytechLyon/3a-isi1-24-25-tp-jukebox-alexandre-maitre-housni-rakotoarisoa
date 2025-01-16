<template>
  <div>
    <Player
      :tracks="tracks"
      @play-track="updateTrackIndex"
    />
    <Playlist
      :tracks="tracks"
      @add-track="addTrack"
      @remove-track="removeTrack"
    />
  </div>
</template>

<script>
import Player from './components/Player.vue';
import Playlist from './components/Playlist.vue';

export default {
  components: { Player, Playlist },
  data() {
    return {
      tracks: [], // Liste des pistes ajoutées
    };
  },
  methods: {
    addTrack(track) {
      this.tracks.push(track);
    },
    removeTrack(index) {
      this.tracks.splice(index, 1);
    },
  },
};
</script>
