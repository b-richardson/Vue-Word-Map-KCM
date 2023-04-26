<template>
    <div>
      <select v-model="selectedCategory">
        <option disabled value="">Please select one</option>
        <option v-for="category in categories" :key="category.id" :value="category.value">{{ category.label }}</option>
      </select>
  
      <label for="startDate">Everything Published since:</label>
      <input id="startDate" type="date" v-model="startDate">
  
      <label for="endDate">Everything Published until:</label>
      <input id="endDate" type="date" v-model="endDate">
  
      <!-- When the "Search" button is clicked, the `searchData` method is called -->
      <button @click="searchData()">Search</button>
  
      <!-- Display a message when the search is being loaded -->
      <p v-if="loading">
        Loading...
      </p>
  
      <!-- Display pick parameters when there is no data to show -->
      <p v-else-if="!info">
        Pick Parameters
      </p>
  
      <!-- Display no results when the search is complete but there is no data returned -->
      <p v-else-if="!info.data || !info.data.content || !info.data.content.length">
        No results to show
      </p>
  
      <!-- Display the fetched data -->
      <template v-else>
        <div>
          <vue-word-cloud
            style="
              height: 680px;
              width: 840px;
            "
            :words="frequencyArr"
            :color="([, weight]) => weight > 10 ? 'DeepPink' : weight > 5 ? 'RoyalBlue' : 'Indigo'"
            font-family="Roboto"
          />
        </div>
      </template>
  
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import VueWordCloud from 'vuewordcloud';
  
  export default {
    data() {
      return {
        info: null,
        selectedCategory: "",
        startDate: "",
        endDate: "",
        categories: [
          { id: 1, label: "Sellers", value: "sellers" },
          { id: 2, label: "Buyers", value: "buyers" },
          { id: 3, label: "Interest Rates", value: "interest-rates" }
        ],
        loading: false,
        frequencyArr: []
      };
    },
    methods: {
      searchData() {
        this.loading = true;
        let url = `https://content.keepingcurrentmatters.com/api/v1/industries/1/content/?types=blog&categories=${this.selectedCategory}&published_since=${this.startDate}&published_since=${this.endDate}`;
  
        axios.get(url, {
          auth: {
            username: "dev_homework",
            password: "vbj2q39rfjksdfqks"
          }
        })
        .then(response => {
          this.info = response;
          this.loading = false;
          console.log(response.data)
  
          const results = response.data.content;
          const wordFreq = results.reduce((map, obj) => {
            const words = obj.contents.split(/\W+/);
            for (let i = 0; i < words.length; i++) {
              const word = words[i].toLowerCase();
  
              if (word in map) {
                map[word]++;
              }
  
              else {
                map[word] = 1;
              }
            }
            return map;
          }, {});
  
          this.frequencyArr = Object.entries(wordFreq)
            .filter(([text, value]) => value > 1 && text.length > 2 && /^[a-zA-Z]+$/.test(text))
            .map(([text, value]) => ([ text, value ]));
            console.log(this.frequencyArr)
        })
  
        .catch(error => {
          console.log(error);
          this.loading = false;
        });
      }
    },
    components: {
      "vue-word-cloud": VueWordCloud
    }
  }
  </script>