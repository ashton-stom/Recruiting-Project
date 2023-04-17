<template>
    <form @submit.prevent="onSubmit">
        <div class="inputsContainer">
            <label>Email</label>
            <input required name='email' type="email" placeholder="email..." v-model="emailText" minLength="1" />
            <label>Tickets</label>
            <input name='tickets' type="number" v-model="tickets" min="1" />
        </div>
        <button class="submit">Submit</button>
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
    border-radius: 4px;
    font-weight: 600;
}

.submit:hover {
    cursor: pointer;
    background-color: #649de7;
}
</style>

<script>

export default {
    data: () => {
        return {
            emailText: "",
            tickets: 0
        };
    },

    props: {
        addGuest: Function
    },

    methods: {
        onSubmit() {
            this.addGuest(this.emailText, this.tickets);
        }
    }
}
</script>