<template>
  <b-modal id="modal-form">
    <form v-if="!isEditing" @submit.prevent="create">
      <input type="e-mail" v-model="guestEmail" name="guestEmail" />
      <input type="number" v-model.number="guestTickets" name="guestTickets" />
      <button type="submit">Add Guest</button>
    </form>
    <form v-else @submit.prevent="update">
      <input type="e-mail" v-model="currentGuest.email" name="guestEmail" />
      <input
        type="number"
        v-model.number="currentGuest.tickets"
        name="guestTickets"
      />
      <button type="submit">Edit Guest</button>
      <button type="reset" @click="cancel">Cancel Edit</button>
    </form>
  </b-modal>
</template>

<script>
// eslint-disable-next-line no-unused-vars
export default {
  props: ["isEditing", "currentGuest", "currentIndex", "showModal"],
  data: () => {
    return {
      guestEmail: "",
      guestTickets: 0,
    };
  },
  created() {
    console.log(this.showModal);
  },
  methods: {
    create() {
      const newGuest = {
        email: this.guestEmail,
        tickets: this.guestTickets,
      };

      this.$emit("add", newGuest);
      this.guestEmail = "";
      this.guestTickets = 0;
    },
    update() {
      const updatedGuest = {
        ...this.currentGuest,
      };

      this.$emit("update", [this.currentIndex, updatedGuest]);
      this.cancel();
    },
    cancel() {
      this.$emit("cancel");
    },
  },
};
</script>

<style scoped>
</style>