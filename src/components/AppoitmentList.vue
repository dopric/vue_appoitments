<template>
  <div class="col-12 col-md-10 col-lg-7">
    <div class="list-group list-group-flush">
      <div
        class="list-group-item d-flex align-items-start"
        v-for="appoitment in appoitments"
        v-bind:key="appoitment.aptId"
      >
        <button class="mr-2 btn btn-sm btn-danger"
        @click="$emit('remove', appoitment)">
          <font-awesome-icon icon="trash"/>
        </button>
        <div class="w-100">
          <div class="d-flex justify-content-between">
            <span class="h4 text-primary"
            contenteditable="contenteditable"
            @blur="$emit('edit', appoitment.aptId, 'petName', $event.target.innerText)">
              {{ appoitment.petName }}</span>
            <span class="float-right">{{ formattedDate(appoitment.aptDate) }}</span>
          </div>
          <div class="owner-name">
            <span class="font-weight-bold text-primary mr-1">Owner:</span>
            <span
            contenteditable="contenteditable"
            @blur="$emit('edit', appoitment.aptId, 'petOwner', $event.target.innerText)">{{ appoitment.petOwner }}</span>
          </div>
          <div 
            contenteditable="contenteditable"
            @blur="$emit('edit', appoitment.aptId, 'aptNotes', $event.target.innerText)">{{ appoitment.aptNotes }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import moment from 'moment';

export default {
  name: "AppoitmentList",
  props: ["appoitments"],
  components: {FontAwesomeIcon},
  methods:{
    formattedDate: function(date){
      return moment(new Date(date)).format("MM-DD-YY h:mm a")
    }
  }
};
</script>

<style></style>
