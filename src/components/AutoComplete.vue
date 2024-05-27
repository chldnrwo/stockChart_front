<template>
    <div class="container mt-5">
      <form @submit.prevent="handleSubmit">
        <div class="row justify-content-center">
          <div class="col-2"></div>
          <div class="col-8">
            <div class="row align-items-center">
              <div class="col-3 d-flex align-items-center justify-content-end">
                <label for="exampleInput" class="form-label mb-0">TICKER</label>
              </div>
              <div class="col-5 autocomplete-container">
                <input
                  type="text"
                  class="form-control"
                  v-model="query"
                  @input="onInput"
                  placeholder="Enter company name"
                />
                <ul v-if="suggestions.length" class="list-group">
                  <li
                    class="list-group-item"
                    v-for="suggestion in suggestions"
                    :key="suggestion.ticker"
                    @click="selectSuggestion(suggestion)"
                  >
                    {{ suggestion.name }}
                  </li>
                </ul>
              </div>
              <div class="col-2">
                <button type="submit" class="btn btn-primary">Submit</button>
              </div>
            </div>
          </div>
          <div class="col-2"></div>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'AutoComplete',
    data() {
      return {
        query: '',
        suggestions: [],
        selectedTicker: '',
      };
    },
    methods: {
      onInput() {
        if (this.query.length > 2) {
          this.fetchSuggestions();
        } else {
          this.suggestions = [];
        }
      },
      async fetchSuggestions() {
        try {
          const response = await axios.get('https://fygmjgopra.execute-api.ap-northeast-2.amazonaws.com/default/polygonTicker', {
            params: {
              q: this.query
            }
          });
  
          let data = response.data.body;
  
          if (typeof data === 'string') {
            data = JSON.parse(data);
          }
  
          if (Array.isArray(data)) {
            this.suggestions = data.slice(0, 8);
          } else {
            console.error('Unexpected response format:', data);
          }
        } catch (error) {
          console.error('Error fetching suggestions:', error);
        }
      },
      selectSuggestion(suggestion) {
        this.query = suggestion.name;
        this.selectedTicker = suggestion.ticker;
        this.suggestions = [];
        this.$emit('company-selected', { ticker: suggestion.ticker, name: suggestion.name });
      },
      handleSubmit() {
        if (!this.selectedTicker) {
          alert('Please select a company from the suggestions.');
          return;
        }
        this.$emit('ticker-selected', { ticker: this.selectedTicker, name: this.query });
      },
    },
  };
  </script>
  
  <style scoped>
  .autocomplete-container {
    position: relative;
  }
  
  .list-group {
    position: absolute;
    z-index: 1000;
    width: 100%;
    background-color: white;
  }
  
  .list-group-item {
    cursor: pointer;
  }
  </style>
  