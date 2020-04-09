<template>
<div class="new-profile">
  <div class="create-profile">
 <h1>Create Profile:</h1>
    <div class="add">
      <div class="form">
        <h2>Profile Picture:</h2>
        <input type="file" name="photo" @change="fileChanged">
        <p></p>
        <input v-model="name" placeholder="Name">
        <p></p>
        <textarea class="bio" v-model="bio" placeholder="Bio"></textarea>
        <p></p>
          <label for="basic-dropdown">Gender: </label>
          <select class="gender" name="basic-dropdown" v-model="selectedGender">
            <option>Male</option>
            <option>Female</option>
            <option>Other</option>
          </select>
          <p></p>
          <label for="basic-dropdown">Month: </label>
          <select class="month" name="basic-dropdown" v-model="selectedMonth">
            <option>January</option>
            <option>Februrary</option>
            <option>March</option>
            <option>April</option>
            <option>May</option>
            <option>June</option>
            <option>July</option>
            <option>August</option>
            <option>September</option>
            <option>October</option>
            <option>November</option>
            <option>December</option>
          </select>
          <label class="day" for="basic-dropdown">Day: </label>
          <select name="basic-dropdown" v-model="selectedDay">
            <option>1</option>
            <option>2</option>
            <option>3</option>
            <option>4</option>
            <option>5</option>
            <option>6</option>
            <option>7</option>
            <option>8</option>
            <option>9</option>
            <option>10</option>
            <option>11</option>
            <option>12</option>
            <option>13</option>
            <option>14</option>
            <option>15</option>
            <option>16</option>
            <option>17</option>
            <option>18</option>
            <option>19</option>
            <option>20</option>
            <option>21</option>
            <option>22</option>
            <option>23</option>
            <option>24</option>
            <option>25</option>
            <option>26</option>
            <option>27</option>
            <option>28</option>
            <option>29</option>
            <option>30</option>
            <option>31</option>
          </select>
          <p></p> 
          <br>
          <button @click="upload">Create Profile</button>
        <center></center>
      </div>
    </div>
  </div>
  <div class="edit-profile">
      <h1>Edit your profile: </h1> 
      <div class="form">
        <select name="basic-dropdown" v-model="selectedProfile">
          <option v-for="prof in listProfiles" :value="prof" :key="prof.id">
            <p> {{ prof.name }} </p>
          </option>
        </select>
        <button class="select-edit" @click="selectProfile(selectedProfile)">Edit Profile</button> 
      </div>
      <p></p>
      <br>
      <div class=profile-info v-if="findProfile">
        <div class="col-2">
          <div class="profile-left">
            <img class="profile-picture" :src="findProfile.path" />
            <button class="edit-button" @click="editProfile(findProfile)">Submit Changes</button>
            <button class="delete-button" @click="editProfile(findProfile)">Delete Profile</button>
          </div>
          <div class="user-info">
            <p class="subtitle">Name:</p>
            <input v-model="findProfile.name" placeholder="Name">
            <p class="subtitle">Bio:</p>
            <textarea class="newBio" v-model="findProfile.bio" placeholder="Bio"></textarea>
            <p class="subtitle"> Gender: </p>
            <select class="gender" name="basic-dropdown" v-model="findProfile.gender">
              <option>Male</option>
              <option>Female</option>
              <option>Other</option>
            </select>
            <p class="subtitle">Birthday:</p>
            <p></p>
          <label for="basic-dropdown">Month: </label>
          <select class="month" name="basic-dropdown" v-model="newSelectedMonth">
            <option>January</option>
            <option>Februrary</option>
            <option>March</option>
            <option>April</option>
            <option>May</option>
            <option>June</option>
            <option>July</option>
            <option>August</option>
            <option>September</option>
            <option>October</option>
            <option>November</option>
            <option>December</option>
          </select>
          <label class="day" for="basic-dropdown">Day: </label>
          <select name="basic-dropdown" v-model="newSelectedDay">
            <option>1</option>
            <option>2</option>
            <option>3</option>
            <option>4</option>
            <option>5</option>
            <option>6</option>
            <option>7</option>
            <option>8</option>
            <option>9</option>
            <option>10</option>
            <option>11</option>
            <option>12</option>
            <option>13</option>
            <option>14</option>
            <option>15</option>
            <option>16</option>
            <option>17</option>
            <option>18</option>
            <option>19</option>
            <option>20</option>
            <option>21</option>
            <option>22</option>
            <option>23</option>
            <option>24</option>
            <option>25</option>
            <option>26</option>
            <option>27</option>
            <option>28</option>
            <option>29</option>
            <option>30</option>
            <option>31</option>
          </select>
          </div> 
        </div>
      </div>
  </div>
</div>

</template>

<script>
import axios from 'axios';
export default {
  name: 'Admin',
  data() {
    return {
      name: "",
      file: null,
      addItem: null,
      findName: "",
      profiles: [],
      findProfile: null,
      selectedProfile: "",
    }
  },
  created() {
    this.getProfiles();
  },
  methods: {
    fileChanged(event) {
      this.file = event.target.files[0]
    },
    async upload() {
      try {
        const formData = new FormData();
        formData.append('photo', this.file, this.file.name)
        let r1 = await axios.post('/api/photos', formData);
        let r2 = await axios.post('/api/items', {
          name: this.name,
          bio: this.bio,
          gender: this.selectedGender,
          birthday: this.selectedMonth.concat(' ', this.selectedDay),
          path: r1.data.path
        });
        this.addItem = r2.data;
      } catch (error) {
        console.log(error);
      }
    },
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
    async editProfile(profile) {
      try {
        await axios.put("api/items/" + profile._id, {
          name: this.findProfile.name,
          bio: this.findProfile.bio,
          gender: this.findProfile.gender,
          birthday: this.newSelectedMonth.concat(' ', this.newSelectedDay),
        });
         this.findProfile = null;
         this.getProfiles();
         return true;
      } catch (error) {
        console.log(error);
      }
    },
    async deleteProfile(profile) {
      try {
        await axios.delete("api/items/" + profile._id);
        this.findProfile = null;
        this.getProfiles();
        return true;
    } catch(error) {
      console.log(error);
    }
    }
  },
  computed: {
    listProfiles() {
      let profiles = this.profiles.filter(item => item.name.toLowerCase().startsWith(this.findName.toLowerCase()));
      return profiles.sort((a, b) => a.name > b.name);
    }
  },
}
</script>

<style scoped>


.create-profile {
  background-color: #343642;
  color: white;
  padding: 20px;
  border: solid black 2px;
  margin-bottom: 20px;
}
.edit-profile {
  background-color: #343642;
  color: white;
  padding: 20px;
  border: solid black 2px;
  min-height: 100px;
}

.image h2 {
  font-style: italic;
  font-size: 1em;
}

.heading {
  margin-bottom: 20px;
  margin-top: 20px;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}



/* Form */
input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}

.form {
  margin-right: 50px;
}
.day {
  margin-left: 10px;
}
.select-edit {
  margin-left: 20px;
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}

 .profile-picture {
  /* max-width: 300px; */
  object-fit: contain;
}

.listProfile {
  min-height: 20px;
}

  /* Profiles */
.col-2 {
  min-height: 300px;
  display: flex;
}
.user-info {
  padding-left: 20px;
}
.profile-left {
  display: flex;
  flex-direction: column;
}
.edit-button {
  margin-top: 40px;
}
.delete-button {
  margin-top: 10px;
}
.subtitle {
  text-decoration: underline;
  font-weight: bold;
}
</style>