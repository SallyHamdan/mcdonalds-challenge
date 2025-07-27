<template>
  <div class="row">
    <div class="col-lg-5 mb-3">
      <input
        type="text"
        class="form-control"
        placeholder="חפש לפי שם"
        v-model="searchText"
        @input="filterStores"
      />
    </div>
    <div class="col-lg-3 mb-3">
      <div class="d-flex align-items-center justify-content-end mb-1">
        <select
          class="form-select"
          v-model="selectedCity"
          aria-label="חפש לפי עיר"
          >
          <option disabled value="">חפש לפי עיר</option>
          <option v-for="city in filteredCities" :key="city" :value="city">{{ city }}</option>
        </select>
        <i class="bi bi-x fs-4 top-50" @click="selectedCity = ''"></i>
      </div>
    </div>
    <div class="col-lg-3 mb-3">
      <div class="d-flex align-items-center justify-content-end mb-1">
        <select
          class="form-select"
          v-model="selectedRegion"
          aria-label="חפש לפי איזור"
          >
          <option disabled value="">חפש לפי איזור</option>
          <option v-for="region in regions" :key="region" :value="region">{{ region }}</option>
        </select>
        <i class="bi bi-x fs-4 top-50" @click="selectedRegion = ''"></i>
      </div>
    </div>
  </div>
  <div class="row">
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">שם סניף</th>
          <th scope="col">כתובת</th>
          <th scope="col">עיר</th>
          <th scope="col">איזור</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="branch in filteredBranches" :key="branch.store_id">
          <th scope="row">{{branch.store_id}}</th>
          <td>{{ branch.store_title}}</td>
          <td>{{ branch.store_address }}</td>
          <td>{{ branch.city }}</td>
          <td>{{ branch.store_region }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  import axios from 'axios';


  export default {
    name: "StoreTable",
    data(){
      return {
        selectedCity: "",
        selectedRegion: "",
        searchText: '',
        branchesData: [],
        cities: [],
        regions: []
      }
    },
    computed: {
      filteredBranches() {
      return this.branchesData.filter(branch => {
        const matchCity = this.selectedCity ? branch.city === this.selectedCity : true;
        const matchRegion = this.selectedRegion ? branch.store_region === this.selectedRegion : true;
        const matchSearch = this.searchText
          ? branch.store_title.toLowerCase().includes(this.searchText.toLowerCase())
          : true;
        return matchCity && matchRegion && matchSearch;
      });
      },
      filteredCities() {
        const filtered = this.selectedRegion
          ? this.branchesData.filter(b => b.store_region === this.selectedRegion)
          : this.branchesData;

        const uniqueCities = [...new Set(filtered.map(b => b.city))];
        console.log(uniqueCities)
        return uniqueCities;
      }
    },
    mounted() {
      this.fetchBranches();
    },
    methods: {
      fetchBranches() {
        axios.get('/api/stores.json')
          .then(response => {
          this.branchesData = response.data;
          this.cities = [...new Set(this.branchesData.map(branch => branch.city))];
          this.regions = [...new Set(this.branchesData
                                      .map(branch => branch.store_region))]
                                      .sort((a, b) => a - b);
        })  
        .catch(error => {
          console.error('error in fetching branches list:', error);
        })
      }
      
    }
  }
</script>

