<template>
  <div>
    <h1>Nyan Cat's Performance</h1>
    <strong>Total Number of Guests:</strong> {{ guestCount }}<br /><br />
    <table>
      <tr>
        <th>E-mail</th>
        <th># of Tickets</th>
        <th>Edit Guest</th>
        <th>Delete Guest</th>
      </tr>
      <tr v-for="(guest, index) in guests" :key="index">
        <td>
          {{ guest.email }}
        </td>
        <td>
          {{ guest.tickets }}
        </td>
        <td>Edit</td>
        <td @click.prevent="deleteGuest(index)">Delete</td>
      </tr>
    </table>
    <br /><br />
    <GuestForm @addNewGuest="addNewGuest" />
    <button>Create New Guest</button>
    <button type="reset" @click.prevent="resetGuests">
      Oops, Deleted All Guests
    </button>
  </div>
</template>

<script>
// eslint-disable-next-line no-unused-vars
import GuestForm from "../components/GuestForm.vue";
const GuestRepository = require("../guest-repository");
const repo = new GuestRepository();

export default {
  components: {
    GuestForm,
  },
  data: () => {
    return {
      guests: [],
      guestCount: 0,
    };
  },
  async created() {
    this.guests = await repo.load();
    this.guestCount = this.guests.length;
  },
  methods: {
    addNewGuest: async function (guest) {
      this.guests = [...this.guests, guest];
      await repo.save(this.guests);
    },
    deleteGuest: async function (index) {
      this.guests.splice(index, 1);
      await repo.save(this.guests);
    },
    resetGuests: async function () {
      this.guests = await repo.reset();
    },
  },
};
</script>
