<template>
  <div>
    <form @submit.prevent="create">
      <input type="e-mail" v-model="guestEmail" name="guestEmail" />
      <input type="number" v-model.number="guestTickets" name="guestTickets" />
      <button type="submit">Add Guest</button>
    </form>
  </div>
</template>

<script>
// eslint-disable-next-line no-unused-vars
export default {
  data: () => {
    return {
      guestEmail: "",
      guestTickets: 0,
    };
  },
  methods: {
    create() {
      const newGuest = {
        email: this.guestEmail,
        tickets: this.guestTickets,
      };

      this.$emit("addNewGuest", newGuest);
      this.guestEmail = "";
      this.guestTickets = 0;
    },
  },
};
</script>

<style>
</style>