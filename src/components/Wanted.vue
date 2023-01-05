<template>
  <h1>Wanted person</h1>
  <div class="identity">
    <h2 style="text-decoration: underline">Identity :</h2>
    <img v-bind:src=" image ">
    <p><strong>Firstname :</strong>  {{ firstname }}</p>
    <p><strong>Lastname : </strong> {{ lastname }}</p>
    <p><strong>Gender : </strong> {{ gender }}</p>
    <p><strong>Date of Birth : </strong> {{ dob }} ({{ age }} years)</p>
  </div>

  <div class="address">
    <h2 style="text-decoration: underline">Last address known: </h2>
    <p><strong>Street : </strong> {{ street }}</p>
    <p><strong>City : </strong> {{ city }}</p>
    <p><strong>State : </strong> {{ state }}</p>
    <p><strong>Country : </strong> {{ country }}</p>
    <p><strong>Postcode : </strong> {{ postcode }}</p>
  </div>

  <div class="info">
    <h2 style="text-decoration: underline">Known information :</h2>
    <p><strong>Email : </strong> {{ email }}</p>
    <p><strong>Cellphone number : </strong> {{ cellphone }}</p>
    <h3>Social media access :</h3>
    <p><strong>Username : </strong> {{ username }}</p>
    <p><strong>Password : </strong> {{ password }}</p>
  </div>
</template>

<script>
import $ from 'jquery';
export default {
  name: "Wanted",
  data() {
    this.getPerson();
    return {
      firstname: "",
      lastname: "",
      image: "",
      gender: "",
      age: "",
      street: "",
      city: "",
      state: "",
      country: "",
      postcode: "",
      email: "",
      username: "",
      password: "",
      dob: "",
      cellphone: "",
    }
  },

  methods: {
    newPerson: async function () {
      return $.ajax({
        url: 'https://randomuser.me/api/',
        dataType: 'json',
      });
    },

    getPerson: async function () {
      const result = await this.newPerson()
      const user = result["results"][0];
      const address = user["location"];
      this.firstname = user["name"]["first"];
      this.lastname = user["name"]["last"];
      this.image = user["picture"]["large"];
      this.gender = user["gender"];
      this.age = user["dob"]["age"];
      this.city = address["city"];
      this.state = address["state"];
      this.country = address["country"];
      this.postcode = address["postcode"];
      this.email = user["email"];
      this.username = user["login"]["username"];
      this.password = user["login"]["password"];
      this.dob = new Date(user["dob"]["date"]).toLocaleDateString();
      this.cellphone = user["cell"];
      this.street = address["street"]["number"] + ' ' + address["street"]["name"];
    }
  }
}
</script>

<style scoped>

</style>