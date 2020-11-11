<template>
  <nuxt-link :to="'detail/' + id">
    <div class="listsearch">
        <hr />
        <div class="row pointer">
          <div class="col-md-3">
            <img
              class="card-img-left img-fluid"
              :src="listsearch.imageUrl"
              alt
            />
          </div>

          <div class="col">
            <h4 class="title">{{listsearch.name}}</h4>

            <p>{{listsearch.description}}</p>
          </div>

          <div class="col-auto">
            <h4>ประกาศของ</h4>
            <p class="card-text">{{ listsearch.tutorName ||'No description provided' }}</p>
            <label for="rating"><h4>Rating:</h4></label>
            <b-form-rating
                  id="rating-inline"
                  inline
                  v-model="listsearch.meanRating"
                  readonly
                  show-value
                  no-border
                  precision="2"
                ></b-form-rating>
          </div>
        </div>
    </div>
  </nuxt-link>
</template>

<script>
export default {
  name: "Listsearch",
  props: ["listsearch", "id"]
};
</script>

<style scoped>
img{
  height: 8rem;
}
</style>
