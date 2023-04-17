<template>
  <div class="container">
    <h3>Total Guests: {{ totalGuests }}</h3>
    <div class="addGuest"><Button class="addGuestButton" :onclick="this.openNewGuestModalForm">Add Guest</Button></div>
    <table>
      <thead>
        <th>Email</th>
        <th>Tickets</th>
      </thead>
      <tbody>
        <tr v-for="(guest, index) in this.guests" :key="index">
          <td>{{ guest.email }}</td>
          <td>{{ guest.tickets }}</td>
          <td class="editButton"><Button
              :onclick="() => selectGuestToEdit({ email: guest.email, tickets: guest.tickets })">Edit</Button></td>
          <td class="deleteX">
            <h4 @click="() => selectGuestToDelete(guest)">X</h4>
          </td>
        </tr>
      </tbody>
    </table>
    <Modal v-if="this.newGuestModalFormIsVisible == true" :onClose="this.closeNewGuestModalForm" title="Add Guest">
      <NewGuestForm :addGuest="this.addGuest" />
    </Modal>
    <Modal v-if="this.deletedGuest" :onClose="this.cancelDelete" title="Delete Guest?">
      <DeleteGuestConfirmation :onConfirm="this.deleteGuest" :onCancel="this.cancelDelete" />
    </Modal>
    <Modal v-if="this.selectedGuestToEdit" :onClose="this.cancelEdit" title="Edit Guest">
      <EditGuestForm :guest="this.selectedGuestToEdit" :updateGuest="this.updateGuest"
        :key="this.selectedGuestToEdit ? this.selectedGuestToEdit.email : 0" :email="this.selectedGuestToEdit.email"
        :tickets="this.selectedGuestToEdit.tickets" />
    </Modal>
  </div>
</template>

<style>
html {
  background-color: #003162;
  color: white;
}

.app {
  height: 90vh;
  padding-top: 16px;
  margin: 36px 0px;
}

.container {
  background-color: #181818;
  height: 100%;
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
}

h3 {
  text-align: center;
  padding-top: 64px;
  margin: 0 auto;
}

.addGuest {
  display: flex;
  width: 75%;
  justify-content: flex-end;
}

.addGuestButton {
  background-color: #00df00;
}

.addGuestButton:hover {
  background-color: #46c446;
}

table {
  width: 50%;
  text-align: center;
  margin: 16px auto;
}

td {
  outline: solid white 2px;
}

.editButton {
  padding: 0 8px;
}

.deleteX {
  padding: 0 8px;
}


h4 {
  color: red;
}

h4:hover {
  cursor: pointer;
}
</style>

<script>
import Button from './Button.vue';
import DeleteGuestConfirmation from './DeleteGuestConfirmation.vue';
import Modal from './Modal.vue';
import NewGuestForm from './NewGuestForm.vue';
import EditGuestForm from './EditGuestForm.vue';

// eslint-disable-next-line no-unused-vars
const GuestRepository = require('../guest-repository');
const guestRepository = new GuestRepository();

export default {
  components: {
    Button,
    Modal,
    NewGuestForm,
    EditGuestForm,
    DeleteGuestConfirmation
  },

  async mounted() {
    this.guests = await guestRepository.load();
  },

  data: () => {
    return {
      guests: [],
      newGuestModalFormIsVisible: false,
      deletedGuest: null,
      selectedGuestToEdit: null,
    };
  },

  computed: {
    totalGuests() {
      let total = 0;
      for (let i = 0; i < this.guests.length; i++) {
        total += this.guests[i].tickets;
      }
      return total;
    }
  },

  methods: {
    openNewGuestModalForm() {
      this.newGuestModalFormIsVisible = true;
    },

    closeNewGuestModalForm() {
      this.newGuestModalFormIsVisible = false;
    },

    async addGuest(email, tickets) {
      const duplicateEmail = this.guests.some((g) => g.email === email);
      if (duplicateEmail) {
        alert('This email already exists!');
        return;
      }
      const totalGuests = this.guests.reduce((total, current) => total += current.tickets, 0)
      if (totalGuests + parseInt(tickets) <= 20) {
        this.guests.push({ email: email, tickets: parseInt(tickets) });
        this.closeNewGuestModalForm();
        await guestRepository.save(this.guests);
      } else {
        alert(`Max occupancy is 20 guests. Current attendance is ${totalGuests}, you can only add up to ${20 - totalGuests} tickets`)
      }
    },

    selectGuestToEdit(guest) {
      this.selectedGuestToEdit = guest;
    },

    cancelEdit() {
      this.selectedGuestToEdit = null;
    },

    cancelDelete() {
      this.deletedGuest = null;
    },

    selectGuestToDelete(guest) {
      this.deletedGuest = guest;
    },

    async updateGuest(updatedGuest) {
      const totalGuests = this.guests.reduce((total, current) => total += current.tickets, 0)
      if (totalGuests + parseInt(updatedGuest.tickets) - this.selectedGuestToEdit.tickets > 20) {
        alert(`Max occupancy is 20 guests. Current attendance is ${totalGuests}, you can only add up to ${20 - totalGuests} tickets`);
        return;
      }
      this.guests = this.guests.map(guest => {
        if (guest.email === this.selectedGuestToEdit.email) {
          return updatedGuest;
        }
        return guest;
      });

      await guestRepository.save(this.guests);
      this.selectedGuestToEdit = null;
    },

    async deleteGuest() {
      const removeGuestIndex = this.guests.findIndex(guest => guest.email === this.deletedGuest.email);
      this.guests.splice(removeGuestIndex, 1);
      this.deletedGuest = null;
      await guestRepository.save(this.guests);
    }
  }
}
</script>
