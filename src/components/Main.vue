<template>
  <div>
    <h1>Nyan Cat's Performance</h1>
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
    <button>Create New Guest</button>
    <button type="reset" @click.prevent="resetGuests">
      Oops, Deleted All Guests
    </button>
  </div>
</template>

<script>
// eslint-disable-next-line no-unused-vars
const GuestRepository = require("../guest-repository");
const repo = new GuestRepository();

export default {
  data: () => {
    return {
      guests: [],
    };
  },
  async created() {
    this.guests = await repo.load();
  },
  methods: {
    addNewGuest: async function (guest) {
      let NewGuestsArray = [...this.guests, guest];
      this.guests = await repo.save(NewGuestsArray);
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
