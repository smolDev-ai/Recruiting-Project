<template>
  <b-modal
    id="modal-form"
    @cancel="
      {
        isEditing ? cancel() : {};
      }
    "
  >
    <b-form v-if="!isEditing" @submit.prevent="create">
      <b-input class="mb-2" type="e-mail" v-model="guestEmail" name="guestEmail" />
      <b-input class="mb-2" type="number" v-model.number="guestTickets" name="guestTickets" />
      <b-button variant="primary" type="submit">Add Guest</b-button>
    </b-form>
    <b-form v-else @submit.prevent="update">
      <b-input class="mb-2" type="e-mail" v-model="currentGuest.email" name="guestEmail" />
      <b-input
        class="mb-2"
        type="number"
        v-model.number="currentGuest.tickets"
        name="guestTickets"
      />
      <b-button class="mr-2" variant="primary" type="submit">Update Guest</b-button>
      <b-button class="mr-2" variant="danger" type="reset" @click="cancel">Cancel Edit</b-button>
    </b-form>
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