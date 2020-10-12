<template>
  <div class="col-12 col-md-10 col-lg-7">
    <div class="list-group list-group-flush">
      <!-- <h4>{{ title }}</h4>
      <i class="fas fa-plus mr-2"></i>Add Appointment -->
      <div
        class="list-group-item d-flex align-items-start"
        v-for="item in appointments"
        :key="item.aptIndex"
      >
        <button
          class="mr-2 btn btn-sm btn-danger"
          @click="$emit('remove', item)"
        >
          <i class="fas fa-trash"></i>
        </button>
        <div class="w-100">
          <div class="d-flex justify-content-between">
            <span
              class="h4# text-primary"
              contenteditable="contenteditable"
              @blur="$emit('edit', item.aptId, 'petName', $event.target.innerText)"
              >{{ item.petName }}</span
            >
            <span class="float-right">{{ formattedDate(item.aptDate) }}</span>
          </div>
          <div class="owner-name">
            <span class="font-weight-bold text-primary mr-1">Owner:</span>
            <span
              contenteditable="contenteditable"
              @blur="$emit('edit', item.aptId, 'petOwner', $event.target.innerText)"
              >{{ item.petOwner }}</span
            >
          </div>
          <div
            contenteditable="contenteditable"
            @blur="$emit('edit', item.aptId, 'aptNotes', $event.target.innerText)"
          >
            {{ item.aptNotes }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: "AppointmentList",
  props: ["appointments"],
  methods: {
    formattedDate(date) {
      return moment(new Date(date)).format("MM-DD-YYYY, h:mm a");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
