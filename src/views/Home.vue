<template>
  <div class="home">
    <h1>Add a New Person</h1>
    <div>
      Name:
      <input type="text" v-model="newPersonName" />
      <br />
      Age:
      <input type="number" v-model="newPersonAge" />
      <br />
      Education level:
      <input type="text" v-model="newPersonEducationLevel" />
      <br />
      Phone Number:
      <input type="text" v-model="newPersonPhoneNumber" />
      <br />
      <button v-on:click="createPerson()">Add a new person</button>
    </div>
    <h1>{{ message }}</h1>
    <div v-for="person in people">
      <h2>{{ person.name }}</h2>
      <button v-on:click="currentPerson = person">details</button>
      <div v-if="person === currentPerson">
        <p>
          At the time {{ person.name }} turned {{ person.age }}, {{ person.education_level }} was their highest level of
          education. For details you can reach them at {{ person.phone }}.
        </p>
      </div>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "People!",
      people: [],
      newPersonName: "",
      newPersonAge: "",
      newPersonPhoneNumber: "",
      newPersonEducationLevel: "",
      currentPerson: {}
    };
  },
  created: function() {
    axios.get("/api/people").then((response) => {
      this.people = response.data;
    });
  },
  methods: {
    createPerson: function() {
      var params = {
        name: this.newPersonName,
        age: this.newPersonAge,
        phone: this.newPersonPhoneNumber,
        education_level: this.newPersonEducationLevel
      };
      axios.post("/api/people", params).then((response) => {
        this.people.push(response.data);
        this.newPersonName = "";
        this.newPersonAge = "";
        this.newPersonEducationLevel = "";
        this.newPersonPhoneNumber = "";
      });
    }
  }
};
</script>
