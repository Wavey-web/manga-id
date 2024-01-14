<template>
  <div>
    <h1>Manga Search</h1>
    <input type="text" v-model="mangaTitle" placeholder="Enter Manga Title">
    <button @click="searchManga">Search</button>
    <div>{{ result }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      mangaTitle: '',
      result: ''
    };
  },
  methods: {
    searchManga() {
      const apiUrl = 'https://api.comick.cc/v1.0/search';
      const queryString = `?limit=49&sort=uploaded&q=${encodeURIComponent(this.mangaTitle)}`;

      fetch(apiUrl + queryString, {
        method: 'GET'
        // Add headers here if needed
      })
      .then(response => {
        if (response.ok) {
          return response.json();
        }
        throw new Error('Network response was not ok.');
      })
      .then(data => {
        // Handle the response data
        const comic = data[0];
        if(comic) {
          this.result = `The HID for ${this.mangaTitle} is: ${comic.hid}`;
        } else {
          this.result = 'No results found.';
        }
      })
      .catch(error => {
        console.error('Error:', error);
        this.result = 'An error occurred.';
      });
    }
  }
};
</script>
