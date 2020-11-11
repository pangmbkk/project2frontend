<template>
  <div class="doneStudent">
    <div v-for="doneStudent in doneStudents" :key="doneStudent.id">
      <hr />
      <div class="row pointer">
        <div class="col-md-3">
          <nuxt-link :to="'detail/' + doneStudent.announcementpost.id">
            <img class="card-img-left img-fluid" :src="doneStudent.announcementpost.imageUrl" alt />
          </nuxt-link>
        </div>

        <div class="col">
          <nuxt-link
            :to="'detail/' + doneStudent.announcementpost.id"
            style="text-decoration: none;
  color: black;"
          >
            <h4 class="title">{{doneStudent.announcementpost.name}}</h4>

            <p>{{doneStudent.announcementpost.description}}</p>
          </nuxt-link>
        </div>

        <div class="col-2-auto">
          <nuxt-link
            :to="'detail/' + doneStudent.announcementpost.id"
            style="text-decoration: none;
  color: black;"
          >
            <h4>ประกาศของ</h4>
            <p
              class="card-text"
            >{{ doneStudent.announcementpost.tutorName ||'No description provided' }}</p>
          </nuxt-link>
          <label for="rating"><h4>Rating:</h4></label>
          <b-form-rating
            id="rating-inline"
            inline
            v-model="doneStudent.announcementpost.meanRating"
            readonly
            show-value
            no-border
            precision="2"
          ></b-form-rating>
        </div>

        <div class="col-auto">
          <b-button   :to="'reviewOrder/' + doneStudent.id" squared variant="info" @click="review()">รีวิว</b-button>
          <!-- <b-button  v-if="doneStudent.rating != null"   squared variant="success" >รีวิวแล้ว</b-button> -->

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
  name: "DoneStudent",
  props: ["doneStudents", "id"],
  data() {
    return {
      // link: `"reviewOrder/"+$route.params.id`
    };
  },
  methods: {
    review() {
      axios
        .post(apiUrl + "/restaurants", {
          name: "chengetest",
          dishes: [
            {
              id: 2,
              name: "kkkk",
              description: "22222",
              price: 20,          
              image: {
                id: 51,
              
              }
            }
          ]
        })
        .then(res => {
          console.log(res);
        });
    }
  }
};
</script>
<style  scoped>
img{
  height: 10rem;
}
</style>
