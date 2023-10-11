<template>
  <div class="app-container">
    <header>
      <p>Dictionary - Vue Application</p>
      <div class="created">created by: <a v-bind:href="'https://www.demjhonsilver.com'" target="_blank">Demjhon Silver</a>
      </div>
    </header>

    <main>
      <div class="search-container">
        <input v-model="searchTerm" @input="searchMeaning" placeholder="Type: happy" class="search-input">
        <button class="search-button" @click="searchMeaning">Search</button>
      </div>
      <div v-if="meaning" class="results">
        <h2 class="word">{{ meaning.word }}</h2>
        <p class="phonetic">Phonetic: {{ meaning.phonetic }}</p>
        <ul v-if="meaning.meanings.length">
          <li v-for="meaningItem in meaning.meanings" :key="meaningItem.partOfSpeech" class="part-of-speech">
            <ul class="meaning-list">
              <li v-for="definition in meaningItem.definitions" :key="definition.definition" class="definition">
                {{ definition.definition }}
                <p v-if="definition.example" class="example">Example: {{ definition.example }}</p>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    </main>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'App',
  setup() {
    const searchTerm = ref('');
    const meaning = ref(null);

    const searchMeaning = async () => {
      if (searchTerm.value) {
        try {
          const response = await fetch(`https://api.dictionaryapi.dev/api/v2/entries/en/${searchTerm.value}`);
          if (response.status === 200) {
            const data = await response.json();
            meaning.value = data[0];
          } else {
            meaning.value = null;
          }
        } catch (error) {
          meaning.value = null;
          console.error("Error fetching data: " + error);
        }
      }
    };

    return { searchTerm, meaning, searchMeaning };
  },
};
</script>
