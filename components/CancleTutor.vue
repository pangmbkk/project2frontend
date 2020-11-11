<template>
  <div class="cancleTutor">
    <div v-for="cancleTutor in cancleTutors" :key="cancleTutor.id">
      <hr />
      <div class="row pointer">
        <div class="col-md-3">
          <nuxt-link :to="'detail/' + cancleTutor.announcementpost.id">
            <img
              class="card-img-left img-fluid"
              :src="cancleTutor.announcementpost.imageUrl"
              alt
            />
          </nuxt-link>
        </div>

        <div class="col">
          <nuxt-link
            :to="'detail/' + cancleTutor.announcementpost.id"
            style="text-decoration: none; color: black"
          >
            <h4 class="title">{{ cancleTutor.announcementpost.name }}</h4>

            <p>{{ cancleTutor.announcementpost.description }}</p>
          </nuxt-link>
        </div>

        <div class="col-2-auto">
          <nuxt-link
            :to="'detail/' + cancleTutor.announcementpost.id"
            style="text-decoration: none; color: black"
          >
            <h4>ประกาศของ</h4>
            <p class="card-text">
              {{
                cancleTutor.announcementpost.tutorName ||
                "No description provided"
              }}
            </p>
          </nuxt-link>
          <label for="rating"><h4>Rating:</h4></label>
          <b-form-rating
            id="rating-inline"
            inline
            v-model="cancleTutor.announcementpost.meanRating"
            readonly
            show-value
            no-border
            precision="2"
          ></b-form-rating>
        </div>

        <div class="col-auto">
          <button class="btn btn-danger">ลบ</button>
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
  name: "CancleTutor",
  props: ["cancleTutors", "id"],
};
</script>
<style  scoped>
img {
  height: 10rem;
}
</style>
