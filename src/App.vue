<template>
  <div id="main-app" class="container">
    <div class="row justify-content-center">
      <add-appointment @add="addAppointment"/>
      <search-appointment @search="searchAppointments" @requestKey="changeKey" @requestDir="changeDir" :myKey="filterKey" :myDir="filterDir"/>
      <appointment-list :appointments="filteredAppointments" @edit="editAppointment" @remove="removeAppointment"/>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import _ from 'lodash';
import AddAppointment from "./components/AddAppointment";
import AppointmentList from "./components/AppointmentList";
import SearchAppointment from "./components/SearchAppointment";

export default {
  name: 'MainApp',
  prop: {
    icon: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      appointments: [],
      appointmentIndex: 0,
      searchTerms: '',
      filterKey: 'aptDate',
      filterDir: 'desc',
    };
  },
  components: {
    AddAppointment,
    AppointmentList,
    SearchAppointment
  },
  mounted() {
    axios.get('./data/appointments.json')
        .then(res => this.appointments = res.data.map(item => {
          item.aptId = ++this.appointmentIndex;
          return item;
        }));
  },
  computed: {
    searchedAppointments: function () {
      return this.appointments.filter(item => {
        return (
            item.petName.toLowerCase().match(this.searchTerms.toLowerCase()) ||
            item.petOwner.toLowerCase().match(this.searchTerms.toLowerCase()) ||
            item.aptNotes.toLowerCase().match(this.searchTerms.toLowerCase())
        );
      });
    },
    filteredAppointments: function () {
      return _.orderBy(this.searchedAppointments, item => {
        return item[this.filterKey].toLowerCase();
      }, this.filterDir);
    }
  },
  methods: {
    addAppointment: function (appointment) {
      appointment.aptId = ++this.appointmentIndex;
      this.appointments.push(appointment);
    },
    removeAppointment: function (appointment) {
      this.appointments = _.without(this.appointments, appointment);
    },
    editAppointment: function (id, field, text) {
      const aptId = _.findIndex(this.appointments, {aptId: id});
      this.appointments[aptId][field] = text;
    },
    searchAppointments: function (terms) {
      this.searchTerms = terms;
    },
    changeKey: function (value) {
      this.filterKey = value;
    },
    changeDir: function (value) {
      this.filterDir = value;
    }
  }
}
</script>
