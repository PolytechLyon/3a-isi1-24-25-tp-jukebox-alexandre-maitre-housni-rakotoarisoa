<template>
  <div class="add-track-form">
    <label for="mode">Add track:</label>
    <select id="mode" v-model="mode">
      <option value="link">By URL</option>
      <option value="file">By file</option>
    </select>

    <!-- Input pour l'ajout par URL -->
    <div v-if="mode === 'link'" class="input-container">
      <input
        v-model="trackLink"
        type="text"
        placeholder="Provide URL"
        aria-label="Track URL"
      />
    </div>

    <!-- Input pour l'ajout par fichier -->
    <div v-if="mode === 'file'" class="input-container">
      <input
        ref="fileInput"
        type="file"
        accept="audio/*"
        aria-label="Upload file"
        @change="handleFileChange"
      />
    </div>

    <button @click="addTrack" :disabled="!isValid">Add</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      mode: 'link', // Mode par défaut : ajout par lien
      trackLink: '', // URL ou titre de la piste
      selectedFile: null, // Fichier sélectionné pour le mode fichier
    };
  },
  computed: {
    isValid() {
      if (this.mode === 'link') {
        return this.trackLink.trim() !== ''; // URL valide
      } else if (this.mode === 'file') {
        return this.selectedFile !== null; // Un fichier a été sélectionné
      }
      return false;
    },
  },
  methods: {
    handleFileChange(event) {
      const fileInput = event.target;
      if (fileInput.files.length > 0) {
        this.selectedFile = fileInput.files[0]; // Stocker le fichier sélectionné
      } else {
        this.selectedFile = null; // Réinitialiser si aucun fichier n'est sélectionné
      }
    },
    addTrack() {
      if (this.mode === 'link' && this.trackLink.trim() !== '') {
        // Ajout par URL
        this.$emit('add-track', {
          title: this.extractTitleFromLink(this.trackLink),
          url: this.trackLink,
          broken: false,
        });
        this.trackLink = ''; // Réinitialiser le champ
      } else if (this.mode === 'file' && this.selectedFile) {
        // Ajout par fichier
        const objectURL = URL.createObjectURL(this.selectedFile);
        this.$emit('add-track', {
          title: this.selectedFile.name,
          url: objectURL,
          broken: false,
        });
        this.selectedFile = null; // Réinitialiser après l'ajout
        this.$refs.fileInput.value = ''; // Réinitialiser l'input fichier
      }
    },
    extractTitleFromLink(link) {
      // Extraire le nom du fichier depuis l'URL
      return link.split('/').pop() || 'Unknown Title';
    },
  },
};
</script>

<style scoped>
.add-track-form {
  margin-bottom: 20px;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>
