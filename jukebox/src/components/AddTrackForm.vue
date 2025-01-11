<template>
  <div class="add-track-form">
    <h3>Add a New Track</h3>
    <form @submit.prevent="addTrack">
      <div>
        <label>Add track:</label>
        <select v-model="mode">
          <option value="link">Via link</option>
          <option value="file">Via file upload</option>
        </select>
      </div>
      <div v-if="mode === 'link'">
        <input v-model="trackLink" type="text" placeholder="Enter track URL" />
      </div>
      <div v-if="mode === 'file'">
        <input ref="fileInput" type="file" accept="audio/*" />
      </div>
      <button type="submit">Add</button>
    </form>
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
