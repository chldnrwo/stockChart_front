<template>
    <div class="autocomplete-container">
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
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'StockAutocomplete',
    data() {
      return {
        query: '',
        suggestions: [],
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

            // response.data는 전체 응답 객체이므로, 실제 데이터는 response.data.body에 있음
            let data = response.data.body;

            // 응답이 JSON 문자열일 경우 파싱
            if (typeof data === 'string') {
            data = JSON.parse(data);
            }

            if (Array.isArray(data)) {
            this.suggestions = data.slice(0, 8); // 상위 8개 결과만 사용
            } else {
            console.error('Unexpected response format:', data);
            }
        } catch (error) {
            console.error('Error fetching suggestions:', error);
        }
      },
      selectSuggestion(suggestion) {
        this.query = suggestion.name;
        this.suggestions = [];
        this.$emit('company-selected', suggestion.ticker);
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
  