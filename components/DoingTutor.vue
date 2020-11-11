<template>
  <div class="doingTutor">
    <div v-for="doingTutor in doingTutors" :key="doingTutor.id">
      <hr />
      <div class="row pointer">
        <div class="col-md-3">
          <nuxt-link :to="'detail/' + doingTutor.postId">
            <img
              class="card-img-left img-fluid"
              :src="doingTutor.announcementpost.imageUrl"
              alt
            />
          </nuxt-link>
        </div>

        <div class="col">
          <nuxt-link
            :to="'detail/' + doingTutor.postId"
            style="text-decoration: none; color: black"
          >
            <h4 class="title">{{ doingTutor.announcementpost.name }}</h4>

            <p>{{ doingTutor.announcementpost.description }}</p>
          </nuxt-link>
        </div>

        <div class="col-2-auto">
          <nuxt-link
            :to="'detail/' + doingTutor.postId"
            style="text-decoration: none; color: black"
          >
            <h4>ประกาศของ</h4>
            <p class="card-text">
              {{
                doingTutor.announcementpost.tutorName ||
                "No description provided"
              }}
            </p>
          </nuxt-link>
          <label for="rating"><h4>Rating:</h4></label>
          <b-form-rating
            id="rating-inline"
            inline
            v-model="doingTutor.announcementpost.meanRating"
            readonly
            show-value
            no-border
            precision="2"
          ></b-form-rating>
        </div>

        <div class="col-auto">
          <button class="btn btn-danger" @click="updateOrder(doingTutor.id)">
            ยกเลิก
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<style>
</style>
<script>
import axios from "axios";
import Strapi from "strapi-sdk-javascript/build/main";
const apiUrl = process.env.API_URL || "http://localhost:1337";
const strapi = new Strapi(apiUrl);
export default {
  name: "DoingTutor",
  props: ["doingTutors", "id"],
  methods: {
    updateOrder(id){
      axios.put(apiUrl + "/orders/"+id ,{
        status:"cancle"
      }).then(res => {
        console.log(res)
        this.$router.go(0)
      })
    }
  },
};
</script>
<style  scoped>
img {
  height: 10rem;
}
</style>
