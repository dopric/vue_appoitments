<template>
  <div id="main-app" class="container">
    <h4>{{ title }}</h4>
    <font-awesome-icon icon="plus" class="mr-2" />Add Appoitment
    <div class="row justify-content-center">
      <AddAppoitment @add="addItem"/>
      <SearchAppoitment @searchRecords="filterAppoitments"/>
      <AppoitmentList v-bind:appoitments="filtered"
      @remove="removeItem"
      @edit="editItem"/>
    </div>
  </div>
</template>

<script>

import axios from "axios";
import AppoitmentList from "./components/AppoitmentList";
import SearchAppoitment from "./components/SearchAppoitment"
import AddAppoitment from './components/AddAppoitment'
import _ from 'loadsh'

export default {
  name: "MainApp",
  data() {
    return {
      title: "Appoitment List",
      appoitments: [],
      aptIndex: 0,
      searchTerms: "",
      filterKey: "petName",
      filterDir: "asc"
    };
  },
  components: {
    AppoitmentList,
    AddAppoitment,
    SearchAppoitment
  },
  mounted() {
    axios
      .get("./data/appoitments.json")
      .then(response => (this.appoitments = response.data.map(item => {
        item.aptId = this.aptIndex;
        this.aptIndex++;
        return item
      })));
  },
  computed: {
    searchedAppoitments: function(){
      console.info("computing properties")
      if (this.searchTerms==="" || this.searchTerms === undefined){
        return this.appoitments;
      }
      // alert("search " + this.searchTerms)
      var filtered =  this.appoitments.filter(item=>{
       console.log(item)
        return(
          item.petName.toLowerCase().match(this.searchTerms.toLowerCase()) 
          ||
          item.petOwner.toLowerCase().match(this.searchTerms.toLowerCase()) ||
          item.aptNotes.toLowerCase().match(this.searchTerms.toLowerCase()) 
        )
      })
      console.log(filtered)
      return filtered;
    },

    filtered: function(){
      return _.orderBy(
        this.searchedAppoitments,
        item=>{
          return item[this.filterKey].toLowerCase();
        }, this.filterDir
      )
    }
  },
  methods: {
    removeItem: function(appoitment){
      this.appoitments = _.without(this.appoitments, appoitment)
    },
    editItem: function(aptId, fieldName, text){
      const aptIndex = _.findIndex(this.appoitments, {
        aptId: aptId
      });
      this.appoitments[aptIndex][fieldName] = text;
    },
    addItem: function(apt){
      apt.aptId = this.aptIndex;
      this.aptIndex++;
      this.appoitments.push(apt)
    },
    filterAppoitments: function(searchTerm){
      this.searchTerms = searchTerm;
    }
  }
};
</script>
