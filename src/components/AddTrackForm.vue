<template>
  <div class="add-track-form">
    <label for="mode">Add track:</label>
    <select id="mode" v-model="mode">
      <option value="link">By URL</option>
      <option value="file">By file</option>
    </select>
    <div v-if="mode === 'link'" class="input-container">
      <input
        v-model="trackLink"
        type="text"
        placeholder="Provide URL"
        aria-label="Track URL"
      />
    </div>
    <div v-if="mode === 'file'" class="input-container">
      <input
        ref="fileInput"
        type="file"
        accept="audio/*"
        aria-label="Upload file"
      />
    </div>
    <button @click.prevent="addTrack" :disabled="!isValid">
      Add
    </button>
  </div>
</template>



<script>
export default {
  data() {
    return {
      mode: 'link', // Choix par défaut : ajout par lien
      trackLink: '',
    };
  },
  methods: {
    addTrack() {
      if (this.mode === 'link' && this.trackLink) {
        this.$emit('add-track', {
          title: this.extractTitleFromLink(this.trackLink),
          url: this.trackLink,
        });
        this.trackLink = ''; // Réinitialiser le champ
      } else if (this.mode === 'file') {
        const fileInput = this.$refs.fileInput;
        if (fileInput.files.length > 0) {
          const file = fileInput.files[0];
          const objectURL = URL.createObjectURL(file);
          this.$emit('add-track', {
            title: file.name,
            url: objectURL,
          });
          fileInput.value = ''; // Réinitialiser le champ
        }
      }
    },
    extractTitleFromLink(link) {
      return link.split('/').pop(); // Extraire le nom depuis l'URL
    },
  },
};
</script>

<style scoped>
/* Style du formulaire d'ajout de piste */
</style>
