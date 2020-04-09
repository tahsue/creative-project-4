<template>
    <div class="container">
        <p class="title">Select your profile:</p>
        <select name="basic-dropdown" v-model="selectedProfile">
          <option v-for="prof in profiles" :value="prof" :key="prof.id">
            <p> {{ prof.name }} </p>
          </option>
        </select>
        <button class="select-profile" @click="selectProfile(selectedProfile)">Select Profile</button>
        <div class="post" v-if="findProfile">
            <p>Hello {{ findProfile.name }}, post something!</p>
            <textarea class="post" v-model="newPost" placeholder="Post"></textarea>
            <p></p>
            <button class="post-submit" @click="postStatus(findProfile)">Submit</button>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: 'Post',
    data() {
        return {
            profiles: [],
            findProfile: null,
        }
    },
    created() {
        this.getProfiles();
    },
    methods: {
        async getProfiles() {
        try {
            let response = await axios.get("/api/items");
            this.profiles = response.data;
            return true;
        } catch(error) {
            console.log(error);
        }
        },
        selectProfile(profile) {
            this.findName = "";
            this.findProfile = profile;
        },
        async postStatus(profile) {
            try {
                await axios.put("api/items/" + profile._id, {
                    post: this.newPost,
                    name: this.findProfile.name,
                    bio: this.findProfile.bio,
                    gender: this.findProfile.gender,
                    birthday: this.findProfile.birthday,
                });
                this.findProfile = null;
                this.getProfiles();
                return true;
            } catch(error) {
                console.log(error);
            }
        },
    }
}
</script>

<style scoped>
.title {
    font-size: 1.3em;
    
}
.container {
    background-color: #343642;
    color: white;
    padding: 20px;
    border: solid black 2px;
    margin-bottom: 20px;

}
.select-profile {
    margin-left: 20px;
}

input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}
</style>