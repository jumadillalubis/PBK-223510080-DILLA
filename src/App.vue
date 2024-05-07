<template>
  <div class="journal-container">
    <h1 style="color: white;">Daily Planner</h1>
    <div class="add-entry">
      <input type="text" v-model="newEntry.text" placeholder="Tambahkan catatan baru..." style="border: 1px solid #ccc; padding: 8px; border-radius: 5px;">
      <select v-model="newEntry.category" style="padding: 8px; border-radius: 5px; margin-left: 10px;">
        <option value="Pribadi">Pribadi</option>
        <option value="Kerja">Kerja</option>
        <option value="Belajar">Belajar</option>
      </select>
      <button @click="addEntry" style="padding: 8px 12px; background-color:#5D001E; color: white; border: none; border-radius: 5px; cursor: pointer; margin-left: 10px;">Tambah</button>
    </div>
    <div v-if="entries.length === 0" class="no-entries" style="background-color: #F08080; padding: 10px; border-radius: 5px;">
      <p>Tidak ada catatan.</p>
    </div>
    <div v-else>
      <div v-for="(category, index) in categories" :key="index" class="category-section">
        <h2 style="color: white;">{{ category }}</h2>
        <ul class="entry-list">
          <li v-for="(entry, i) in filteredEntries(category)" :key="i" class="entry-item" style="border-bottom: 1px solid #ccc; padding: 10px 0;">
            <span>{{ entry.text }}</span>
            <span v-if="entry.favorite" class="favorite-indicator" style="color: red; margin-left: 10px;">Favorite</span>
            <div class="entry-buttons" style="margin-left: auto;">
              <button @click="toggleFavorite(entry)" class="favorite-button" style="padding: 6px 10px; background-color: #EE4C7C; color: white; border: none; border-radius: 5px; cursor: pointer;">{{ entry.favorite ? 'Batal Favorit' : 'Favoritkan' }}</button>
              <button @click="deleteEntry(entry)" class="delete-button" style="padding: 6px 10px; background-color: #E3AFBC; color: white; border: none; border-radius: 5px; cursor: pointer; margin-left: 5px;">Hapus</button>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      entries: [
        { text: "Catatan pribadi pertama", category: "Pribadi", favorite: false },
        { text: "Catatan kerja pertama", category: "Kerja", favorite: false },
        { text: "Catatan belajar pertama", category: "Belajar", favorite: false }
      ],
      newEntry: { text: '', category: 'Pribadi', favorite: false }
    };
  },
  computed: {
    categories() {
      return Array.from(new Set(this.entries.map(entry => entry.category)));
    }
  },
  methods: {
    addEntry() {
      if (this.newEntry.text.trim() !== '') {
        this.entries.push({ ...this.newEntry });
        this.newEntry.text = '';
        this.newEntry.category = 'Pribadi';
      }
    },
    deleteEntry(entry) {
      const index = this.entries.indexOf(entry);
      if (index !== -1) {
        this.entries.splice(index, 1);
      }
    },
    toggleFavorite(entry) {
      entry.favorite = !entry.favorite;
    },
    filteredEntries(category) {
      return this.entries.filter(entry => entry.category === category);
    }
  }
};
</script>

<style scoped>
.journal-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #000000;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.add-entry {
  margin-bottom: 20px;
  display: flex;
  align-items: center;
}

.entry-list {
  list-style-type: none;
  padding: 0;
}

.entry-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.no-entries {
  text-align: center;
}

</style>
