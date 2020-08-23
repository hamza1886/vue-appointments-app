<template>
  <div class="col-12 col-md-10 col-lg-7">
    <div class="list-group list-group-flush">
      <div class="list-group-item d-flex align-items-start" v-for="appointment in appointments" :key="appointment.aptId">
        <button class="btn btn-danger btn-sm mr-2" @click="$emit('remove', appointment)" title="Delete Appointment">
          <font-awesome-icon icon="trash"/>
        </button>
        <div class="w-100">
          <div class="d-flex justify-content-between">
            <span class="h4 text-primary" contenteditable @blur="$emit('edit', appointment.aptId, 'petName', $event.target.innerText)">{{ appointment.petName }}</span>
            <span class="float-right">{{ formattedDate(appointment.aptDate) }}</span>
          </div>
          <div class="owner-name">
            <span class="text-primary font-weight-bold mr-1">Owner:</span>
            <span contenteditable @blur="$emit('edit', appointment.aptId, 'petOwner', $event.target.innerText)">{{ appointment.petOwner }}</span>
          </div>
          <div contenteditable @blur="$emit('edit', appointment.aptId, 'aptNotes', $event.target.innerText)">{{ appointment.aptNotes }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import moment from 'moment';

export default {
  name: 'AppointmentList',
  props: ["appointments"],
  components: {
    FontAwesomeIcon
  },
  methods: {
    formattedDate: function (date) {
      return moment(new Date(date)).format('DD-MM-YYYY, h:mm a');
    }
  }
}
</script>
