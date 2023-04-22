<template>
  <div>
    <h1>Nyan Cat's Performance</h1>
    <strong>Total Number of Guests:</strong> {{ guestCount }} of
    {{ maxGuestCount }}<br /><br />
    <b-table-simple hover bordered>
      <b-thead>
        <b-tr>
          <b-th>E-mail</b-th>
          <b-th># of Tickets</b-th>
          <b-th colspan="2">Actions</b-th>
        </b-tr>
      </b-thead>
      <b-tbody>
        <b-tr v-for="(guest, index) in guests" :key="index">
          <b-td>
            {{ guest.email }}
          </b-td>
          <b-td>
            {{ guest.tickets }}
          </b-td>
          <b-td colspan="2">
            <b-button
              squared
              variant="primary"
              v-b-modal.modal-form
              class="mr-2"
              @click.prevent="toggleEdit(index)"
              >Edit</b-button
            >
            <b-button
              squared
              variant="danger"
              @click.prevent="deleteGuest(index)"
              >Delete</b-button
            >
          </b-td>
        </b-tr>
      </b-tbody>
    </b-table-simple>
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
    <b-button class="mr-2" variant="success" v-b-modal.modal-form>Create New Guest</b-button>
    <b-button variant="danger" type="reset" @click.prevent="resetGuests">
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
      let total = 0;
      for (let guest of this.guests) {
        total += guest.tickets;
      }

      this.guestCount = total;
    },
    addNewGuest: async function (guest) {
      this.guests = [...this.guests, guest];
      if (this.guestCount <= this.maxGuestCount) {
        this.getGuestCount();
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
      this.getGuestCount()
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
      this.guests = await repo.load();
    },
  },
};
</script>
