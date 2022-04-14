<template>
  <b-container fluid>
    <!-- User Interface controls -->
      <b-col lg="6" class="my-1">
        <b-form-input v-model="value"
      placeholder="enter your country"
      required
    ></b-form-input><br /><br />
    <button @click="getData()">submit</button><br><br>
        <b-form-group
          label="Filter"
          label-for="filter-input"
          label-cols-sm="3"
          label-align-sm="right"
          label-size="sm"
          class="mb-0"
        >
          <b-input-group size="sm">
            <b-form-input
              id="filter-input"
              v-model="filter"
              type="search"
              placeholder="Type to Search"
            ></b-form-input>

            <b-input-group-append>
              <b-button :disabled="!filter" @click="filter = ''">Clear</b-button>
            </b-input-group-append>
          </b-input-group>
        </b-form-group>
        </b-col>
      <b-col sm="5" md="6" class="my-1">
        <b-form-group
          label="Per page"
          label-for="per-page-select"
          label-cols-sm="6"
          label-cols-md="4"
          label-cols-lg="3"
          label-align-sm="right"
          label-size="sm"
          class="mb-0"
        >
        <b-form-checkbox-group
            v-model="filterOn"
            :aria-describedby="ariaDescribedby"
            class="mt-1"
          >
            <b-form-checkbox value="webpages">"web_pages"</b-form-checkbox>
            <b-form-checkbox value="alpha_two_code">alpha_two_code</b-form-checkbox>
            <b-form-checkbox value="country">country</b-form-checkbox>
            <b-form-checkbox value="name">name</b-form-checkbox>
            <b-form-checkbox value="domains">domains</b-form-checkbox>
            <b-form-checkbox value="state-province">state-province</b-form-checkbox>
            </b-form-checkbox-group>

            <b-container >
            <b-card-text v-for="cdata in posts" :key="cdata.id">
            <b-row>
            <b-col>{{ cdata.name }} </b-col>
            </b-row>
            <b-row> <b-col>{{ cdata.domains }}</b-col>
            <b-col>{{ cdata.web_pages }}</b-col>
            </b-row>
            </b-card-text>
            </b-container>
           
          <b-form-select
            id="per-page-select"
            v-model="perPage"
            :options="pageOptions"
            size="sm"
          ></b-form-select>
        </b-form-group>
      </b-col>

      <b-col sm="7" md="6" class="my-1">
        <b-pagination
          v-model="currentPage"
          :total-rows="totalRows"
          :per-page="perPage"
          align="fill"
          size="sm"
          class="my-0"
        ></b-pagination>
      </b-col>

    <!-- Main table element -->
    <b-table
      :items="posts"
      :fields="fields"
      :current-page="currentPage"
      :per-page="perPage"
      :filter="filter"
      :filter-included-fields="filterOn"
      stacked="md"
      show-empty
      small
      @filtered="onFiltered"
    >
      <!--<template #cell(name)="row">
        {{ row.value.first }} {{ row.value.last }}
      </template>-->

      <template #cell(actions)="row">
        <b-button size="sm" @click="info(row.item, row.index, $event.target)" class="mr-1">
          Info modal
        </b-button>
        <b-button size="sm" @click="row.toggleDetails">
          {{ row.detailsShowing ? 'Hide' : 'Show' }} Details
        </b-button>
      </template>

      <template #row-details="row">
        <b-card>
          <ul>
            <li v-for="(value, key) in row.item" :key="key">{{ key }}: {{ value }}</li>
          </ul>
        </b-card>
      </template>
    </b-table>

    <!-- Info modal -->
    <b-modal :id="infoModal.id" :title="infoModal.title" ok-only @hide="resetInfoModal">
      <pre>{{ infoModal.content }}</pre>
    </b-modal>
  </b-container>
</template>

<script>
  export default {
    name:'asNe',
    data() {
      return {
         posts: " ",
         fields: [
        "web_pages",
        "alpha_two_code",
        "country",
        "name",
        "domains",
        "state-province",
      ],
        totalRows: 1,
        currentPage: 1,
        perPage: 5,
        pageOptions: [5, 10, 15, { value: 100, text: "Show a lot" }],
        filter: null,
        filterOn: [],
        infoModal: {
          id: 'info-modal',
          title: '',
          content: ''
      }
    }
    },
  methods: {
    async getData() {
      try {
        let response = await fetch(
          "http://universities.hipolabs.com/search?country=" + this.value
        );
        this.posts = await response.json();
      } catch (error) {
        console.log(error);
      }
    },
  },

    mounted() {
      // Set the initial number of items
      this.totalRows = this.items.length
    },
      onFiltered(filteredItems) {
        // Trigger pagination to update the number of buttons/pages due to filtering
        this.totalRows = filteredItems.length
        this.currentPage = 1
      }
    }

</script>