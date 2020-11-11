<template>
  <div class="container-fluid text-center">
    <div>
      <b-form-group>
        <!-- prop `add-on-change` is needed to enable adding tags vie the `change` event -->
        <b-form-tags
          v-model="value"
          size="sm"
          add-on-change
          no-outer-focus
          class="mb-2"
        >
          <template
            v-slot="{
              tags,
              inputAttrs,
              inputHandlers,
              disabled,
              removeTag,
            }"
          >
            <b-form-select
              v-bind="inputAttrs"
              v-on="inputHandlers"
              :disabled="disabled || availableOptions.length === 0"
              :options="availableOptions"
            >
              <template v-slot:first>
                <!-- This is required to prevent bugs with Safari -->
                <option disabled value="">Choose a tag...</option>
              </template>
            </b-form-select>

            <ul v-if="tags.length > 0" class="list-inline d-inline-block mb-2">
              <li v-for="tag in tags" :key="tag" class="list-inline-item">
                <b-form-tag
                  @remove="removeTag(tag)"
                  :title="tag"
                  :disabled="disabled"
                  variant="info"
                  >{{ tag }}</b-form-tag
                >
              </li>
            </ul>
          </template>
        </b-form-tags>
      </b-form-group>
    </div>
    <p>{{ value }}</p>
    <p>{{ value2 }}</p>

    <button class="btn btn-primary" @click="test(value)">click</button>
    <b-form-timepicker v-model="value3" locale="en"></b-form-timepicker>
    <div class="mt-2">Value3: {{ value3 }}</div>
    <button class="btn btn-primary" @click="test2(value3)">click</button>
  </div>
</template>

<script>
import Strapi from "strapi-sdk-javascript/build/main";
import axios from "axios";
const apiUrl = process.env.API_URL || "http://localhost:1337";
const strapi = new Strapi(apiUrl);
export default {
  data() {
    return {
      options: [
        "จันทร์",
        "อังคาร",
        "พุธ",
        "พฤหัสบดี",
        "ศุกร์",
        "เสาร์",
        "อาทิตย์",
      ],
      value: [],
      value2: "",
      value3: null,
    };
  },
  computed: {
    availableOptions() {
      return this.options.filter((opt) => this.value.indexOf(opt) === -1);
    },
  },
  methods: {
    test(v) {
      if (!v?.length == true) {
        console.log("null5555");
      }
      strapi
        .getEntries("announcementposts", {
          day: this.value,
          meanRating_gte: this.meanRating_gte,
          meanRating_lt: this.meanRating_lt,
        })
        .then((res) => {
          this.value2 = res; // res มันคือ data แล้ว
          console.log(res);
        });
      // axios.get( "http://localhost:1337/announcementposts?day=" +
      //       this.value).then( res=>{
      //          console.log(res.data)

      //     this.value2= res.data // res มันคือ data แล้ว
      //       })
      //47
    },
    test2(v3) {
      if (v3 == null) {
        console.log("time null");
      }
      // strapi.updateEntry('announcementposts',"5f47d3a1abce411a418484cc",{timetostart:this.value3}).then(res2 => {
      //       console.log(res2)
      //   })
    },
  },
};
</script>

<style>
</style>