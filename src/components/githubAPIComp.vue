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
        <div><p>Total number of repositories : {{ totNumRepo }}</p></div>
        <div><p>Link to user: <a v-bind:href="userURL" target="_blank">{{ userURL }}</a></p></div>
        <ul>
            <!-- Loop through the api request -->
            <li v-for="project in projects" :key="project.id">
                <p>{{ project.name }}</p>
                <p>{{ project.description }}</p>
                <p>Created on: {{ project.created_at }}</p>
                <a v-bind:href="project.clone_url" target="_blank">Link: {{ project.clone_url }}</a>
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
            newUser: '',
            totNumRepo: '_',
            userURL: ''
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
                    //Use sorting method to sort data cronologically
                    const cronoUserSort = response.data.sort((a,b) => {
                        //create dates a and b objects to compare against eachother
                        const dateA = new Date(a.created_at);
                        const dateB = new Date(b.created_at);
                        //We subtract A from B to determine a numerical number
                        //If the returned value is negative, a will be placed before b.
                        //If the returned value is positive, a will be placed after b.
                        return dateB - dateA;
                    })
                    
                    this.projects = cronoUserSort
                    this.totNumRepo = cronoUserSort.length
                    this.userURL = cronoUserSort['0'].owner.html_url
                    let printData = false
                    if(printData == true){
                        for(let i = 0; i < cronoUserSort.length; i++){
                        console.log(cronoUserSort[i].created_at);
                    }
                    
                    }
                    
                })
                .catch(error => {
                    console.log(error);
                });
        },
    },
};
</script>