<template>
  <div class="waitingpost">
    <div v-for="waitingpost in waitingposts" :key="waitingpost.id">
      <hr />
      <div class="row pointer">
        <div class="col-md-3">
          <nuxt-link :to="'detail/' + waitingpost.announcementpost.id">
            <img class="card-img-left img-fluid" :src=" waitingpost.announcementpost.imageUrl" alt />
          </nuxt-link>
        </div>

        <div class="col">
          <nuxt-link
            :to="'detail/' + waitingpost.announcementpost.id"
            style="text-decoration: none;
  color: black;"
          >
            <h4 class="title">{{waitingpost.announcementpost.name}}</h4>

            <p>{{waitingpost.announcementpost.description}}</p>
          </nuxt-link>
        </div>

        <div class="col-auto">
          <nuxt-link
            :to="'detail/' + waitingpost.announcementpost.id"
            style="text-decoration: none;
  color: black;"
          >
            <label for="tutorname"><h4>ประกาศของ</h4></label>
            <p
              class="card-text"
            >{{ waitingpost.announcementpost.tutorName ||'No description provided' }}</p>
          </nuxt-link>
          <label for="rating"><h4>Rating:</h4></label>
          <b-form-rating
            id="rating-inline"
            inline
            v-model="waitingpost.announcementpost.meanRating"
            readonly
            show-value
            no-border
            precision="2"
          ></b-form-rating>
        </div>

        <div class="col-auto">
          <b-button squared variant="danger" @click="deleteorder(waitingpost.id)">ลบ</b-button>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
</style>
<script>
import axios from "axios";
import Strapi from "strapi-sdk-javascript/build/main";
const apiUrl = process.env.API_URL || "http://localhost:1337";
const strapi = new Strapi(apiUrl);
export default {
  name: "Waitingpost",
  props: ["waitingposts", "id"],
  data() {
    return {
      modalShow: false,
    };
  },
  methods: {
    updatepost() {
      console.log("sssss");
    },
    async deleteorder(ids) {
      axios.delete(apiUrl + "/orders/" + ids).then((res) => {
        console.log("deletepos success" + ids);
        console.log(res);
      });
      location.reload();
    },
  },
};
</script>
<style  scoped>
img{
  height: 10rem;
}
</style>

