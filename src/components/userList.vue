<template>
    <transition-group tag="ul" name="list-item">
    <li v-for="user in users" :key="user" @click="removeUser(user)"> {{user}}</li>
    </transition-group>
    
    <input type="text" ref="inputUser"/>
    <button @click="addUser">Add</button>
</template>

<script>
export default {
    name: "userList",
    data() {
        return {
            users: ['Max','Julie', 'John']
        }
    },
    methods: {
        addUser() {
            const username = this.$refs.inputUser.value;
            this.users.unshift(username)
        },
        removeUser(user) {
            this.users = this.users.filter(usr => usr!==user)
        }
    }
}
</script>

<style scoped>
ul {
    margin: 1rem 0;
    padding: 0;
    list-style: none;
}

li {
    padding: 1rem;
    border: 1px solid #ccc;
    text-align: center;
}

input {
    margin-bottom: 10px;
}

.list-item-enter-from {
    opacity: 0;
    transform: translateX(-30px);
}

.list-item-enter-active {
    transition: all 1s ease-out;
}

.list-item-enter-to,
.list-item-leave-from {
    opacity: 1;
    transform: translateX(0);
}

.list-item-leave-active {
    transition: all 1s ease-in;
}

.list-item-leave-to {
    opacity: 0;
    transform: translateX(30px);
}
</style>