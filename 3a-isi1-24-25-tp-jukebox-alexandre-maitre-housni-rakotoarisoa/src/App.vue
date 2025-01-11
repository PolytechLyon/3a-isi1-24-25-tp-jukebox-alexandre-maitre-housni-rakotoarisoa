<template>
  <div id="jukebox">
    <Player />
    <Playlist />
    <AddTrackForm />
  </div>
</template>

<script>
import Player from './components/Player.vue';
import Playlist from './components/Playlist.vue';
import AddTrackForm from './components/AddTrackForm.vue';

export default {
  components: {
    Player,
    Playlist,
    AddTrackForm,
  },
};
</script>

<style scoped>
#jukebox {
  font-family: Arial, sans-serif;
  margin: 20px;
}
</style>
