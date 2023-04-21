<template>
  <div>
    <h1>Nyan Cat's Performance</h1>
    <strong>Total Number of Guests:</strong> {{ guestCount }} of
    {{ maxGuestCount }}<br /><br />
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
        <b-button v-b-modal.modal-form @click.prevent="toggleEdit(index)"
          >Edit</b-button
        >
        <b-button class="danger" @click.prevent="deleteGuest(index)"
          >Delete</b-button
        >
      </tr>
    </table>
    <br /><br />
    <GuestForm
      :currentIndex="currentIndex"
      :currentGuest="currentGuest"
      :isEditing="isEditing"
      :showModal="showModal"
      @add="addNewGuest"
      @update="udpateGuest"
      @cancel="cancelEdit"
    />
    <b-button v-b-modal.modal-form>Create New Guest</b-button>
    <b-button class="danger" type="reset" @click.prevent="resetGuests">
      Oops, Deleted All Guests
    </b-button>
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
      showModal: true,
      isEditing: false,
      maxGuestCount: 20,
      currentGuest: {
        guestEmail: "",
        guestTickets: 0,
      },
      currentIndex: 0,
    };
  },
  async created() {
    this.guests = await repo.load();
    this.getGuestCount();
  },
  methods: {
    getGuestCount: function () {
      this.guestCount = this.guests.length;
      for (let guest of this.guests) {
        this.guestCount += guest.tickets;
      }
    },
    addNewGuest: async function (guest) {
      if (this.guestCount !== this.maxGuestCount) {
        this.guests = [...this.guests, guest];
        await repo.save(this.guests);
      }
    },
    toggleEdit: function (index) {
      this.isEditing = true;
      this.guests = this.guests.map((guest, idx) => {
        if (index !== idx) {
          return guest;
        }

        this.currentGuest = guest;
        this.currentIndex = index;
        return guest;
      });
    },
    udpateGuest: async function (index, updatedGuest) {
      this.guests = this.guests.map((guest, idx) => {
        if (index !== idx) {
          return guest;
        }

        return updatedGuest;
      });
      await repo.save(this.guests);
    },
    cancelEdit: function () {
      this.isEditing = false;
    },
    deleteGuest: async function (index) {
      this.guests.splice(index, 1);
      this.getGuestCount();
      await repo.save(this.guests);
    },
    resetGuests: async function () {
      this.guests = await repo.reset();
    },
  },
};
</script>
