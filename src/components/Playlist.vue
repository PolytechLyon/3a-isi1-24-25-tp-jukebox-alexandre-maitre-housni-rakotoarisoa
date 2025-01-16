<template>
  <div class="playlist">

    <!-- Formulaire d'ajout -->
    <AddTrackForm @add-track="addTrack" />

    <!-- Liste des pistes -->
    
  </div>
</template>

<script>
import AddTrackForm from './AddTrackForm.vue';

export default {
  components: {
    AddTrackForm,
  },
  props: {
    tracks: {
      type: Array,
      required: true,
    },
  },
  methods: {
    addTrack(track) {
      this.$emit('add-track', track);
    },
    removeTrack(index) {
      this.$emit('remove-track', index);
    },
  },
};
</script>

<style scoped>
.playlist {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>
