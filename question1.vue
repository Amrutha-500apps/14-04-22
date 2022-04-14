<template>
<div>
  <h2>Question-1</h2>
  <h3> Fetch tha data from the given URL</h3>
  <h3>Implementation of Search functionality</h3>
<b-form-input v-model="text" placeholder="Please Enter Country Name" required>
</b-form-input><br><br>
<button @click="fatchdata()">Submit</button><br><br>
<center><input type="text" name="search" id="search" v-on:keyup="search()" placeholder="search .."></center><br>
<center> <table id="mytable">
<thead>
<tr>
<th><b> University Name </b></th>
<th><b> Domains</b></th>
<th><b> Web pages</b></th>
</tr>
</thead>
<tbody>
<tr v-for="data1 in posts" :key="data1.id">
<td>{{data1.name}} </td>
<td>{{data1.domains}}</td>
<td>{{data1.web_pages}}</td>
</tr>
</tbody>
</table>
</center>
<button @click="getfun1()">filtername</button><br><br>
<div class="container">
<div class="row justify-content-center">
<div class="col-md-8">
<input type='text1' v-model="search1" placeholder='Filter name...'/><br>
<ul>
<li v-for="person1 in filteredPeople1" :key="person1.id">
{{ person1.name }}
</li>
</ul>
</div>
</div>
</div>
<button @click="getfun2()">filter website</button><br><br>
<div class="container">
<div class="row justify-content-center">
<div class="col-md-8">
<input type='text1' v-model="search2" placeholder='Filter Website...'/><br>
<ul>
<li v-for="person2 in filteredPeople2" :key="person2.id">
{{ person2. web_pages}}
</li>
</ul>
</div>
</div>
</div>
<button @click="getfun3()">filtercountry</button><br><br>
<div class="container">
<div class="row justify-content-center">
<div class="col-md-8">
<input type='text1' v-model="search3" placeholder='Filter country...'/><br>
<ul>
<li v-for="person3 in filteredPeople3" :key="person3.id">
{{ person3.country}}
</li>
</ul>
</div>
</div>
</div>
</div>
</template>
<script>
export default{
    name:"Question5AA",
    data(){
      return{
        //fields:["web_pages","alpha_two_code","country","name","domains","state-province"],
         posts:" ",
         people: [],
         search1: '',
         search2:" ",
         search3:" "
        }
    },
    methods:{
     async fatchdata(){
       try{
           let response=await fetch("http://universities.hipolabs.com/search?country="+this.text);
           this.posts=await response.json();
          }
        catch(error){
        console.log(error);
        }
    },
     search() {
     var searchbar, filter, table, tr, td, i, txt;
     searchbar = document.getElementById("search");
     filter =searchbar.value.toUpperCase();
     table = document.getElementById("mytable");
     tr = table.getElementsByTagName("tr");
     for (i = 0; i < tr.length; i++) {
     td = tr[i].getElementsByTagName("td")[0];
     if (td) {
     txt = td.textContent || td.innerText;
     if (txt.toUpperCase().indexOf(filter) > -1) {
     tr[i].style.display = "";
     } else {
     tr[i].style.display = "none";
     }
    }
   }
  },
   getfun1() {
   fetch("http://universities.hipolabs.com/search?country")
   .then(response=>response.json())
   .then(data=>{
   this.people=data;
   })
  },
   getfun2() {
   fetch("http://universities.hipolabs.com/search?country")
   .then(response=>response.json())
   .then(data=>{
   this.people=data;
   })
   },
   getfun3() {
   fetch("http://universities.hipolabs.com/search?country")
   .then(response=>response.json())
   .then(data=>{
   this.people=data;
   })
   }
  },
   created(){
   this.fatchdata();
   },
    computed: {
    filteredPeople1: function() {
     return this.people.filter((person1) => {
     return person1.name.match(this.search1);
    });
   },
    filteredPeople2:function() {
    return this.people.filter((person2) => {
    return person2.name.match(this.search2);
    });
    },
    filteredPeople3:function() {
    return this.people.filter((person3) => {
    return person3.name.match(this.search3);
    });
   }
  }
}
</script>




