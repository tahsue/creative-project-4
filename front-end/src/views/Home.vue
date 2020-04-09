<template>
  <div class="home">
    <div class='profiles' v-for="prof in profiles" :key="prof.id">
      <div class="profile">
        <div class="columns">
          <div class="column">
            <img class="profile-picture" :src="prof.path" />
          </div>
          <div class="column-info">
            <p class="name"> {{ prof.name }} </p>
            <p class="subtitle">About Me:</p>
            <p class="bio"> {{ prof.bio }} </p>
            <div class="status" v-if="prof.post">
              <p class="subtitle">Status:</p>
              <p class="currentStatus">{{ prof.post }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';
export default {
  name: 'Home',
  data() {
    return {
      profiles: [],
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
  }
}
</script>


<style scoped>

.columns {
  display: flex;
}
.column-info {
  margin-left: 50px;
}

.profile {
  border: solid black 2px;
  background-color: #343642;
  color: white;
  margin-top: 30px;
  padding: 30px;
}
.profile-picture {
  border: solid white 2px;
  padding: 10px;
}
.name {
  font-size: 30px;
  font-weight: bold;
}
.bio {
  font-size: 15px;
}
.subtitle {
  font-weight: bold;
  text-decoration: underline;
}
</style>
