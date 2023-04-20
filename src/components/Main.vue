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
        <td>Delete</td>
      </tr>
    </table>
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
    return this.guests;
  },
};
</script>
