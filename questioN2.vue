<template>
<div>
  <h3 align="center">Country List</h3>
  <b-container>
  <b-form-select v-model="value" :options="countries"></b-form-select><br>
  <b-button variant="primary" @click="getData" id="sample">Search</b-button><br><br>
<b-pagination v-model="currentPage" :total-rows="rows" :per-page="perPage" aria-controls="my-table"></b-pagination>
<b-row cols-md="7">
<b-table id="my-table" :per-page="perPage" :current-page="currentPage" striped hover :items="items">
<b-col><p>university_name: {{ value.university_name }}</p></b-col>
<b-col><p>domains: {{ value.domains }}</p></b-col>
<a :href="value.website_url" _target="blank">{{value.website_url}}</a>
  <!-- <p @click="redirect(value.website_url)">
  website_url: {{ value.website_url }}
  </p> -->
  <!-- <template slot="actions" >
  <button class="btn btn-dark" @click="redirect(value.website_url)" :ref="'btn'">website_url: {{value.website_url}}</button>
  </template> -->
<b-col><p>state_province: {{ value.state_province }}</p></b-col>
</b-table>
</b-row>
</b-container>
<b-container mt=5>
  <!-- <div class="row">
  <div class="col form-inline">
  <b-form-input v-model="newtask" placeholder="Enter Newtask" @keyup.enter="add"></b-form-input><b-button class="ml-2" variant="primary" @click="add">Add</b-button>
  </div>
  </div> -->
<div class="row mt=3">
<div class="col-md-3">
<div class="p-2 alert alert-secondary">
<h3>Country List</h3>
<draggable class="list-group kanban-column" :list="countries" group="tasks" >
<div class="list-group-item" v-for="value in items" :key="value.domains">
<!-- <b-pagination v-model="currentPage" :total-rows="rows" :per-page="perPage" aria-controls="sample2"></b-pagination> -->
<b-card id="sample2" v-for="value in items" :key="value.domains" class="mb-2">
<b-card-text>university_name: {{ value.university_name }} </b-card-text>
<p>domains: {{ value.domains }}</p>
<p @click="redirect(value.website_url)">
website_url: {{ value.website_url }}</p>
<p>state_province: {{ value.state_province }}</p>
</b-card>
</div>
</draggable>
</div>
</div>
<!-- <div class="col-md-3">
<div class="p-2 alert alert-primary">
<h3>ArrBackLog</h3>
<draggable class="list-group kanban-column" :list="arrInprogress" group="tasks">
<div class="list-group-item" v-for="element in arrInprogress" :key="element.name">
{{element.name}}
</div>
</draggable>
</div>
</div>
<div class="col-md-3">
<div class="p-2 alert alert-success">
<h3>ArrBackLog</h3>
<draggable class="list-group kanban-column" :list="arrTested" group="tasks">
<div class="list-group-item" v-for="element in arrTested" :key="element.name">
{{element.name}}
</div>
</draggable>
</div>
</div> -->
<div class="col-md-3">
<div class="p-2 alert alert-success">
  <h3>Country List2</h3>
<draggable class="list-group kanban-column" :list="countries" id="sample" group="tasks" >
<div class="list-group-item" v-for="value in items" :key="value.domain">
</div>
</draggable>
</div>
</div>
<div class="col-md-3">
<div class="p-2 alert alert-primary">
  <h3>Country List3</h3>
<draggable class="list-group kanban-column" :list="countries" id="sample" group="tasks" >
<div class="list-group-item" v-for="value in items" :key="value.domain">
</div>
</draggable>
</div>
</div>
</div>
</b-container>
</div>
</template>
<script>
const { getNames } = require("country-list");
import draggable from "vuedraggable";
  export default {
     name: "QuE1",
  data() {
     return {
        perPage:3,
        currentPage:1,
        value: "",
        items: [],
        countries: [],
               // newtask:"",
              // arrBacklog:[{name:"code signup Page"},
             // {name:"Test Dashboard"},
            // {name:"style registration"},
           // {name:"Help with Desk"}, ],
          // arrInprogress:[],
         // arrTested:[],
        // arrDone:[],
     };
  },
 components:{
   draggable
  },
    mounted() {
     let countries = getNames();
     this.countries = countries.map((row) => {
     return { value: row, text: row };
     });
     //if (this.countries.length) this.value = this.countries[0].text;
    },
    methods: {
     redirect(value) {
     window.open(value, "_blank");
    },
     async getData() {
     const response = await fetch(
     "http://universities.hipolabs.com/search?country=" + this.value,
    {
     method: "GET",
    }
   );
     const responseText = await response.json();
    this.items = responseText.map((row) => {
      return {
      university_name: row.name,
      domains: row.domains,
      website_url: row.web_pages[0],
      state_province: row["state-province"],
      };
   });
   },
    // add(){
   // if(this.newtask){
   // this.arrBacklog.push({name: this.newtask});
  // this.newtask=""
  // }
  // }
  },
   computed:{
    rows(){
      return this.items.length;
    }
   }
  };
</script>
<style scoped>
.kanban-column{
min-height:300px;
}
</style>