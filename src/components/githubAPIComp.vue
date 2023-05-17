<template>
    <div>
        <h1>Github API requester</h1>
        <div>
            <label for='name'>Type in a user name</label>
        </div>
        <div>
            <input v-model="newUser" type="text" placeholder="Type in user name">
            <button @click="fetchProjects">Submit</button>
        </div>
        <ul>
            <!-- Loop through the api request -->
            <li v-for="project in projects" :key="project.id">
                <p>{{ project.name }}</p>
            </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            projects: [],
            newUser: ''
        };
    },
    mounted() {
        //this.fetchProjects();
    },
    methods: {
        fetchProjects() {
            const userName = this.newUser;
            axios
                .get(`https://api.github.com/users/${userName}/repos`)
                .then(response => {
                    this.projects = response.data;
                })
                .catch(error => {
                    console.log(error);
                });
        },
    },
};
</script>