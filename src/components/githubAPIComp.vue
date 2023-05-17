<template>
    <div>
        <h1>Github API requester</h1>
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
        };
    },
    mounted() {
        this.fetchProjects();
    },
    methods: {
        fetchProjects() {
            const userName = 'Arran-Logan-Riley';

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