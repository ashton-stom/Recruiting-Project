<template>
    <form>
        <div class="inputsContainer">
            <label>Email</label>
            <input name='email' type="text" placeholder="email..." v-model="emailInput" />
            <label>Tickets</label>
            <input name='tickets' type="number" v-model="ticketsInput" />
        </div>
        <Button class="submit" :onclick="this.onSubmit">Submit</Button>
    </form>
</template>

<style>
form {
    width: 100%;
    text-align: center;
    color: black;
    font-weight: 600;
}

.inputsContainer {
    width: 80%;
    margin: 0 auto;
}

input {
    margin: 0px 4px;
}

.submit {
    margin: 32px auto 16px auto;
}

.submit:hover {
    cursor: pointer;
}
</style>

<script>
import Button from './Button.vue';

export default {
    data() {
        return {
            emailInput: this.email,
            ticketsInput: this.tickets
        }
    },

    components: {
        Button
    },

    props: {
        updateGuest: Function,
        email: String,
        tickets: Number,
        guest: Object
    },

    methods: {
        onSubmit(e) {
            e.preventDefault();
            this.updateGuest({ email: this.emailInput, tickets: parseInt(this.ticketsInput) });
        }
    }
}
</script>