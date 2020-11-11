<template>
  <div class="container">
    <!-- <div>{{ profile }}</div> -->
    <h2 class="text-center">แก้ไขโปรไฟล์</h2>
    <form>
      <div class="form-group">
        <label for="exampleInputEmail1"
          >Email : <b>{{ email }}</b></label
        >
      </div>
      <div class="form-group">
        <label for="exampleInputPassword1">ชื่อ</label>
        <input type="fname" class="form-control" id="fname" v-model="fname" />
      </div>
      <div class="form-group">
        <label for="exampleInputPassword1">นามสกุล</label>
        <input type="lname" class="form-control" id="lname" v-model="lname" />
      </div>
      <div class="form-group">
        <label for="exampleInputPassword1">เบอร์ติดต่อ</label>
        <input type="phone" class="form-control" id="phone" v-model="phone" />
      </div>
      <div class="form-group">
        <label for="exampleInputPassword1">ประวัติการศึกษา</label>
        <input
          type="education"
          class="form-control"
          id="education"
          v-model="education"
        />
      </div>

      <button
        type="submit"
        class="btn btn-primary"
        v-on:click.stop.prevent="onEdit()"
      >
        แก้ไข
      </button>
    </form>
  </div>
</template>

<script>
import Strapi from "strapi-sdk-javascript/build/main";
import axios from "axios";
const apiUrl = process.env.API_URL || "http://localhost:1337";
const strapi = new Strapi(apiUrl);
export default {
  computed: {
    tutoruser() {
      return this.$store.getters["auth/tutoruser"];
    },
  },
  data() {
    return {
      fname: "",
      lname: "",
      email: "",
      phone: "",
      education: "",
      profile: "",
    };
  },
  async created() {
    axios
      .get("http://localhost:1337/users/" + this.tutoruser.id)
      .then((response) => {
        this.profile = response.data;
        this.email = response.data.email;
        this.fname = response.data.fname;
        this.lname = response.data.lname;
        this.phone = response.data.phone;
        this.education = response.data.education;
        console.log("user profile");
      });
  },
  methods: {
    onEdit() {
      axios
        .put("http://localhost:1337/users/" + this.tutoruser.id, {
          fname: this.fname,
          lname: this.lname,
          phone: this.phone,
          education: this.education,
        })
        .then((response) => {
          console.log(response);
          this.$router.back();
        });
    },
  },
};
</script>

<style>
</style>