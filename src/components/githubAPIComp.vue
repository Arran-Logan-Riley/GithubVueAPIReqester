<template>
    <div>
        <h1>Github API Requester</h1>
        <div>
            <label for='name'>Type in a user name</label>
        </div>
        <div class="">
            <input class="input box" v-model="newUser" type="text" placeholder="Type in user name">
            <button class="button" @click="fetchProjects">Submit</button>
        </div>
        <div><p>Total number of repositories : {{ totNumRepo }}</p></div>
        <div><p>Link to user: <a v-bind:href="userURL" target="_blank">{{ userURL }}</a></p></div>
        <ul>
            <!-- Loop through the api request -->
            <li v-for="project in projects" :key="project.id">
                <h3>{{ project.name }}</h3>
                <p>{{ project.description }}</p>
                <p>Created on: {{ project.created_at }}</p>
                <a v-bind:href="project.clone_url" target="_blank">Link: {{ project.clone_url }}</a>
                <hr>
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
                    //If there is more than 30 repos, change it to 30+. Cannot request more than 30 repos.
                    if(cronoUserSort.length == 30){
                        this.totNumRepo = "30+"
                    }else{
                        this.totNumRepo = cronoUserSort.length
                    }
                    
                    this.userURL = cronoUserSort['0'].owner.html_url
                    
                    let printData = false
                    if(printData == true){
                        for(let i = 0; i < cronoUserSort.length; i++){
                        console.log(cronoUserSort[i].created_at);
                    }
                    
                    }
                    
                })
                .catch(error => {
                    this.userURL = 'No user found';
                    this.totNumRepo = 0;
                    console.log(error);
                });
        },
    },
};
</script>

<style>
body{
    margin: 0; 
    padding: 0;
    background: #19161c;
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    flex-direction: column;
    align-content: center;
    color: white;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
}
.box{
    position: relative;
}
.input {
    padding: 10px;
    width: 300;
    height: 40px;
    background: none;
    border: 2px solid #ffd52d;
    border-radius: 50px;
    box-sizing: border-box;
    font-size: 26px;
    color: #ffd52d;
    outline: none;
    transition: .5s;
}

.button{
    background: none;
    margin: 10px;
    height: 40px;
    border: 2px solid #ffd52d;
    border-radius: 50px;
    box-sizing: border-box;
    font-size: 26px;
    color: #ffd52d;
    outline: none;
    transition: .5s;
}

a{
    color: #ffeaa7
}

.button:hover{
    background: #3b3640;
    border-radius: 10px;
}

.input:hover{
    background: #3b3640;
    border-radius: 10px;
}

#app{
width: 90%;
}
</style>