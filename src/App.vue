<template>
  <div id="app" class="container">
    <div class="row justify-content-center">
      <nasa-search :nasaData="nasaData"/>
      <img :src="imgSrc" :title="imgTitle" />
      <AddAppointment @add="addItem" />
      <SearchAppointments
        @searchRecords="searchAppointments"
        :myKey="filterKey"
        :myDir="filterDir"
        @keyChange = "changeKey"
        @dirChange = "changeDir"
      />
      <AppointmentList
        :appointments="filteredApts"
        @remove="removeItem"
        @edit="editItem"
      />
    </div>
  </div>
</template>

<script>
import AppointmentList from "./components/AppointmentList";
import AddAppointment from "./components/AddAppointment";
import SearchAppointments from "./components/SearchAppointments";
import NasaSearch from "./components/NasaSearch"
import _ from "lodash";
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      appointments: [],
      filterKey: "petName",
      filterDir: "asc",
      searchTerms: "",
      aptIndex: 0,
      nasaData: [],
      imgTitle: "",
      imgSrc: ""

    };
  },
  components: {
    AppointmentList,
    SearchAppointments,
    AddAppointment,
    NasaSearch
  },
  mounted() {
    axios.get("./data/appointments.json").then(
      (response) =>
        (this.appointments = response.data.map((item) => {
          item.aptId = this.aptIndex;
          this.aptIndex++;
          return item;
        }))
    );
    var apiKey = 'Jy0x5WUNcruYwdKk5AtDkBY6eWmKluKUm8AWKJuo'
      var url = 'https://api.nasa.gov/EPIC/api/natural/images?api_key='+ apiKey
      axios.get(url).then(res => {
        this.nasaData = res.data
      })
  },
  computed: {
    searchedApts() {
      return this.appointments.filter((item) => {
        return (
          item.petName.toLowerCase().match(this.searchTerms.toLowerCase()) ||
          item.petOwner.toLowerCase().match(this.searchTerms.toLowerCase()) ||
          item.aptNotes.toLowerCase().match(this.searchTerms.toLowerCase())
        );
      });
    },
    filteredApts() {
      return _.orderBy(
        this.searchedApts,
        (item) => {
          return item[this.filterKey].toLowerCase();
        },
        this.filterDir
      );
    },
  },
  methods: {
    searchAppointments(terms) {
      this.searchTerms = terms;
    },
    addItem(apt) {
      apt.aptId = this.aptIndex;
      this.aptIndex++;
      this.appointments.push(apt);
    },
    removeItem(apt) {
      this.appointments = _.without(this.appointments, apt);
    },
    editItem(id, field, text) {
      const aptIndex = _.findIndex(this.appointments, {
        aptId: id,
      });
      this.appointments[aptIndex][field] = text;
    },
    changeKey: function(value) {
      this.filterKey = value;
    }, //changeKey
    changeDir: function(value) {
      this.filterDir = value;
    }, //changeKey
  },
};
</script>
