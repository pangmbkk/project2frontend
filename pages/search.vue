<template>
  <div class="bodynon">
    <div class="container-fluid row">
      <div class="container">
        <br />
        <form @submit.prevent="onSearch()">
          <div class="form-group row">
            <!-- ฟอรม เลือก จังหวัด-->
            <div class="col-1"></div>

            <div class="col-4">
              <b-form-select v-model="province" size="sm">
                <option :value="null">--เลือกจังหวัดของคุณ--</option>
                <option
                  v-for="province in provinces"
                  :key="province.id"
                  :value="province.name"
                >
                  {{ province.name }}
                </option>
              </b-form-select>
            </div>
            <div class="col-4">
              <b-form-select v-model="subjectname" size="sm">
                <option :value="null">--เลือกวิชา--</option>
                <option
                  v-for="subjectname in subjectnames"
                  :key="subjectname.id"
                  :value="subjectname.name"
                >
                  {{ subjectname.name }}
                </option>
              </b-form-select>
            </div>
            <div class="col"></div>
          </div>
          <!-- จบform -->
          <div class="form-group row">
            <div class="col-1"></div>

            <div class="col-4">
              <b-form-select v-model="educationlevel" size="sm">
                <option :value="null">--ระดับชั้น--</option>
                <option
                  v-for="educationlevel in educationlevels"
                  :key="educationlevel.id"
                  :value="educationlevel.name"
                >
                  {{ educationlevel.name }}
                </option>
              </b-form-select>
            </div>
            <div class="col-4">
              <b-form-select v-model="experience" size="sm">
                <option :value="null">--เลือกประสบการณ์ผู้สอน--</option>
                <option
                  v-for="experience in experiences"
                  :key="experience.id"
                  :value="experience.name"
                >
                  {{ experience.name }}
                </option>
              </b-form-select>
            </div>

            <button
              class="btn btn-primary btn-sm col"
              v-on:click.stop.prevent="
                onSearch(
                  province,
                  subjectname,
                  educationlevel,
                  experience,
                  timetostart,
                  timetoend,
                  value,
                  meanRating
                )
              "
            >
              ค้นหา
            </button>
          </div>
          <div class="form-group row">
            <!-- ฟอรม เลือก จังหวัด-->
            <div class="col-1"></div>

            <div class="col-4">
              <div>
                <b-form-timepicker
                  v-model="timetostart"
                  locale="en"
                  size="sm"
                  placeholder="เวลาเริ่มต้น HH:mm:ss"
                ></b-form-timepicker>
              </div>
            </div>
            <div class="col-4">
              <div>
                <b-form-timepicker
                  v-model="timetoend"
                  locale="en"
                  size="sm"
                  placeholder="เวลาสิ้นสุด HH:mm:ss"
                ></b-form-timepicker>
              </div>
            </div>
          </div>
          <div class="form-group row">
            <!-- ฟอรม เลือก จังหวัด-->
            <div class="col-1"></div>

            <div class="col-4">
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
                          <option disabled value="">--เลือกวันที่ติว--</option>
                        </template>
                      </b-form-select>

                      <ul
                        v-if="tags.length > 0"
                        class="list-inline d-inline-block mb-2"
                      >
                        <li
                          v-for="tag in tags"
                          :key="tag"
                          class="list-inline-item"
                        >
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
            </div>
            <div class="col-4">
              <div>
                <b-form-select v-model="meanRating" size="md">
                  <option :value="null">--เลือกตามคะแนนเรตติ้ง--</option>
                  <option
                    v-for="meanRating in meanRatings"
                    :key="meanRating.id"
                    :value="meanRating"
                  >
                    {{ meanRating.name }}
                  </option>
                </b-form-select>
                <p>{{ meanRating }}</p>
              </div>
            </div>
          </div>
        </form>
      </div>
      <!--จบcontainer -->
    </div>
    <!--จบcontainer-fluid -->

    <div class="container-fluid body">
      <div class="container">
        <br />
        <div class="row">
          <div class="col-md-9">
            <div>
              <Listsearch
                class="router"
                v-for="listsearch in listsearchs"
                :key="listsearch.id"
                :id="listsearch.id"
                :listsearch="listsearch"
              />
            </div>
          </div>

          <div class="col-md-3">
            <hr />
            <ul class="list-group">
              <li class="list-group-item texbox">
                <h5>วิธีการใช้งานเว็บไซต์</h5>
                <div class="">
                  <li style="list-style-type: none">
                    <span
                      class="service-icon rounded-circle text-center"
                      style="align: left"
                    >
                      <font-awesome-icon fas icon="user-plus" />
                    </span>
                    <span class="ml-2">ลงทะเบียน</span>
                  </li>
                  <li style="list-style-type: none">
                    <span
                      class="service-icon rounded-circle text-center"
                      style="align: left"
                    >
                      <font-awesome-icon fas icon="sign-in-alt" />
                    </span>
                    <span class="ml-2">ล๊อกอินเข้าสู่ระบบ</span>
                  </li>
                  <li style="list-style-type: none">
                    <span
                      class="service-icon rounded-circle text-center"
                      style="align: left"
                    >
                      <font-awesome-icon fas icon="mouse" />
                    </span>
                    <span class="ml-2">สร้าง/เลือกโพสประกาศ</span>
                  </li>
                  <li style="list-style-type: none">
                    <span
                      class="service-icon rounded-circle text-center"
                      style="align: left"
                    >
                      <font-awesome-icon fas icon="reply" />
                    </span>
                    <span class="ml-2">รอการตอบรับ</span>
                  </li>
                </div>
              </li>
            </ul>
            <br />
            <ul class="list-group">
              <li class="list-group-item texbox">
                <h5>วิธีการใช้งานเว็บไซต์</h5>
                <div class="">
                  <li style="list-style-type: none">
                    <span
                      class="service-icon rounded-circle text-center"
                      style="align: left"
                    >
                      <font-awesome-icon fas icon="user-plus" />
                    </span>
                    <span class="ml-2">ลงทะเบียน</span>
                  </li>
                  <li style="list-style-type: none">
                    <span
                      class="service-icon rounded-circle text-center"
                      style="align: left"
                    >
                      <font-awesome-icon fas icon="sign-in-alt" />
                    </span>
                    <span class="ml-2">ล๊อกอินเข้าสู่ระบบ</span>
                  </li>
                  <li style="list-style-type: none">
                    <span
                      class="service-icon rounded-circle text-center"
                      style="align: left"
                    >
                      <font-awesome-icon fas icon="mouse" />
                    </span>
                    <span class="ml-2">สร้าง/เลือกโพสประกาศ</span>
                  </li>
                  <li style="list-style-type: none">
                    <span
                      class="service-icon rounded-circle text-center"
                      style="align: left"
                    >
                      <font-awesome-icon fas icon="reply" />
                    </span>
                    <span class="ml-2">รอการตอบรับ</span>
                  </li>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
.router {
  text-decoration: none;
  color: black;
}

.bodynon {
  background-color: #2c3e50 !important;
}
.texbox {
  background-color: none;
}
.body {
  background-color: white;
}
img {
  margin-bottom: 5px;
  margin-top: 5px;
  border: 0;
  width: 50;
  max-width: 100%;
  height: auto;
}
.pointer {
  cursor: pointer;
}

.service-icon {
  background-color: #fff;
  color: #1d809f;
  height: 3rem;
  width: 3rem;

  line-height: 3.5rem;
  font-size: 1rem;
  box-shadow: 0 3px 3px 0 rgba(0, 0, 0, 0.1);
}
</style>

<script>
// @ is an alias to /src
import Strapi from "strapi-sdk-javascript/build/main";
import axios from "axios";
const apiUrl = process.env.API_URL || "http://localhost:1337";
const strapi = new Strapi(apiUrl);
import Listsearch from "../components/Listsearch";

export default {
  components: {
    Listsearch,
  },

  data() {
    return {
      listsearchs: [],
      infos: [],
      info: {
        id: "0",
        name: "",
        description: "",
      },
      provinces: [],
      province: null,
      subjectnames: [],
      subjectname: null,
      educationlevels: [],
      educationlevel: null,
      experiences: [],
      experience: null,
      meanRatings: [],
      meanRating: null,
      allOrders: "",
      timetostart: null,
      timetoend: null,
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
    };
  },
  computed: {
    availableOptions() {
      return this.options.filter((opt) => this.value.indexOf(opt) === -1);
    },
  },
  async created() {
    axios.get("http://localhost:1337/announcementposts").then((response) => {
      this.listsearchs = response.data;
      this.allOrders = response.data.orders; //order
      console.log("get Announcementposts success");
    });
    axios.get("http://localhost:1337/provinces").then((res) => {
      this.provinces = res.data;
      console.log("get provinces success");
    });
    axios.get("http://localhost:1337/subjectnames").then((res) => {
      this.subjectnames = res.data;
      console.log("get subjectnames success");
    });
    axios.get("http://localhost:1337/educationlevels ").then((res) => {
      this.educationlevels = res.data;
      console.log("get educationlevels success");
    });
    axios.get("http://localhost:1337/experiences ").then((res) => {
      this.experiences = res.data;
      console.log("get experiences success");
    });
    axios.get("http://localhost:1337/mean-ratings").then((res) => {
      this.meanRatings = res.data;
      console.log("get experiences success");
    });
  },
  methods: {
    async onSearch(
      searchedprovicetype,
      searchedsubjecttype,
      searchededucationleveltype,
      searchedexperiencetype,
      searchtimetostart,
      searchtimetoend,
      searchvalue,
      searchmeanRating
    ) {
      if (
        //1
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get("http://localhost:1337/announcementposts")
          .then((response) => {
            this.listsearchs = response.data;

            console.log("get onS.allapiAnnouncementposts success");
          });
      } else if (
        //2
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName=" +
              this.province
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch province success");
          });
        return;
      } else if (
        //3
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?subjectName=" +
              this.subjectname
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch subjectname success");
          });
        return;
      } else if (
        //4
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?educationName=" +
              this.educationlevel
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch educationlevel success");
          });
        return;
      } else if (
        //5
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?experienceName=" +
              this.experience
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch experience success");
          });
        return;
      } else if (
        //6
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?timetostart=" +
              this.timetostart
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch timetostart success");
          });
        return;
      } else if (
        //7
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?timetoend=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch timetoend success");
          });
        return;
      } else if (
        //8
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", { day: this.value })
          .then((res) => {
            this.listsearchs = res; //คือdataแล้ว
            console.log("get onsearch day success");
          });
        return;
      } else if (
        //9
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            meanRating_gte: this.meanRating.gte,
            meanRating_lt: this.meanRating.lt,
          })
          .then((res) => {
            this.listsearchs = res; //คือdataแล้ว
            console.log("get onsearch meanRating success");
          });
        return;
      } else if (
        //10  get2ตัวแปร
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?subjectName_in=" +
              this.subjectname +
              "&provinceName_in=" +
              this.province
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch province subjectname success both");
          });
        return;
      } else if (
        //11
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?educationName_in=" +
              this.educationlevel +
              "&provinceName_in=" +
              this.province
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch province educationlevel success both");
          });
        return;
      } else if (
        //12
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?experienceName_in=" +
              this.experience +
              "&provinceName_in=" +
              this.province
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch province experience success both");
          });
        return;
      } else if (
        //13
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?timetostart_in=" +
              this.timetostart +
              "&provinceName_in=" +
              this.province
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch province timetostart success both");
          });
        return;
      } else if (
        //14
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?timetoend_in=" +
              this.timetoend +
              "&provinceName_in=" +
              this.province
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch province timetoend success both");
          });
        return;
      } else if (
        //15
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            provinceName: this.province,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch province day success both");
          });
        return;
      } else if (
        //16
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt +
              "&provinceName_in=" +
              this.province
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch province meanrating success both");
          });
        return;
      } else if (
        //17
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?educationName_in=" +
              this.educationlevel +
              "&subjectName_in=" +
              this.subjectname
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch subjectname educationlevel success both");
          });
        return;
      } else if (
        //18
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?experienceName_in=" +
              this.experience +
              "&subjectName_in=" +
              this.subjectname
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch subjectname experience success both");
          });
        return;
      } else if (
        //19
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?timetostart_in=" +
              this.timetostart +
              "&subjectName_in=" +
              this.subjectname
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch subjectname timetostart success both");
          });
        return;
      } else if (
        //20
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?timetoend_in=" +
              this.timetoend +
              "&subjectName_in=" +
              this.subjectname
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch subjectname timetoend success both");
          });
        return;
      } else if (
        //21
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            subjectName: this.subjectname,
          })
          .then((res) => {
            this.listsearchs = res;
            console.log("get onsearch subjectname day success both");
          });

        return;
      } else if (
        //22
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt +
              "&subjectName_in=" +
              this.subjectname
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch subjectname meanrating success both");
          });
        return;
      } else if (
        //23
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?experienceName_in=" +
              this.experience +
              "&educationName_in=" +
              this.educationlevel
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch educationlevel experience success both");
          });
        return;
      } else if (
        //24
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?timetostart_in=" +
              this.timetostart +
              "&educationName_in=" +
              this.educationlevel
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch educationlevel timetostart success both");
          });
        return;
      } else if (
        //25
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?timetoend_in=" +
              this.timetoend +
              "&educationName_in=" +
              this.educationlevel
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch educationlevel timetoend success both");
          });
        return;
      } else if (
        //26
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            educationName: this.educationlevel,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch educationlevel day success both");
          });
        return;
      } else if (
        //27
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt +
              "&educationName_in=" +
              this.educationlevel
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch educationlevel meanrating success both");
          });
        return;
      } else if (
        //28
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?timetostart_in=" +
              this.timetostart +
              "&experienceName_in=" +
              this.experience
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch experience timetostart success both");
          });
        return;
      } else if (
        //29
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?timetoend_in=" +
              this.timetoend +
              "&experienceName_in=" +
              this.experience
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch experience timetoend success both");
          });
        return;
      } else if (
        //30
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            experienceName: this.experience,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch experience day success both");
          });
        return;
      } else if (
        //31
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt +
              "&experienceName_in=" +
              this.experience
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get onsearch experience meanrating success both");
          });
        return;
      } else if (
        //32
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?timetoend_in=" +
              this.timetoend +
              "&timetostart_in=" +
              this.timetostart
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get timetostart  timetoend  success both");
          });
        return;
      } else if (
        //33
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            timetostart: this.timetostart,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get timetostart  day  success both");
          });
        return;
      } else if (
        //34
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt +
              "&timetostart_in=" +
              this.timetostart
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get timetostart  meanrating  success both");
          });
        return;
      } else if (
        //35
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get timetoend  day  success both");
          });
        return;
      } else if (
        //36
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt +
              "&timetoend_in=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log("get timetoend  meanrating  success both");
          });
        return;
      } else if (
        //37
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            meanRating_gte: this.meanRating.gte,
            meanRating_lt: this.meanRating.lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get timetoend  meanrating  success both");
          });
        return;
      } else if (
        //38  3api
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&educationName_in=" +
              this.educationlevel
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname educationlevel success 3api"
            );
          });
        return;
      } else if (
        //39
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&experienceName_in=" +
              this.experience
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname experience success 3api"
            );
          });
        return;
      } else if (
        //40
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&timetostart_in=" +
              this.timetostart
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname timetostart success 3api"
            );
          });
        return;
      } else if (
        //41
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&timetoend_in=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname timetoend success 3api"
            );
          });
        return;
      } else if (
        //42
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            provinceName: this.province,
            subjectName: this.subjectname,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch province subjectname day success 3api");
          });
        return;
      } else if (
        //43
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname timetoend success 3api"
            );
          });
        return;
      } else if (
        //44
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&educationName_in=" +
              this.educationlevel +
              "&experienceName_in=" +
              this.experience
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province educationlevel experience success 3api"
            );
          });
        return;
      } else if (
        //45
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&educationName_in=" +
              this.educationlevel +
              "&timetostart_in=" +
              this.timetostart
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province educationlevel timetostart success 3api"
            );
          });
        return;
      } else if (
        //46
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&educationName_in=" +
              this.educationlevel +
              "&timetoend_in=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province educationlevel timetoend success 3api"
            );
          });
        return;
      } else if (
        //47
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            provinceName: this.province,
            educationName: this.educationlevel,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel day success 3api"
            );
          });
        return;
      } else if (
        //48
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&educationName_in=" +
              this.educationlevel +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province educationlevel meanRating success 3api"
            );
          });
        return;
      } else if (
        //49
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&experienceName_in=" +
              this.experience +
              "&timetostart_in=" +
              this.timetostart
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province experience timetostart success 3api"
            );
          });
        return;
      } else if (
        //50
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&experienceName_in=" +
              this.experience +
              "&timetoend_in=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province experience timetoend success 3api"
            );
          });
        return;
      } else if (
        //51
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            provinceName: this.province,
            experienceName: this.experience,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch province experience day success 3api");
          });
        return;
      } else if (
        //52
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&experienceName_in=" +
              this.experience +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province experience meanRating success 3api"
            );
          });
        return;
      } else if (
        //53
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&timetostart_in=" +
              this.timetostart +
              "&timetoend_in=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province timetostart timetoend success 3api"
            );
          });
        return;
      } else if (
        //54
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            provinceName: this.province,
            timetostart: this.timetostart,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch province timetostart day success 3api");
          });
        return;
      } else if (
        //55
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&timetostart_in=" +
              this.timetostart +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province timetostart meanRating success 3api"
            );
          });
        return;
      } else if (
        //56
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            provinceName: this.province,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch province timetoend day success 3api");
          });
        return;
      } else if (
        //57
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&timetoend_in=" +
              this.timetoend +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province timetoend meanRating success 3api"
            );
          });
        return;
      } else if (
        //58
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            meanRating_gte: this.meanRating.gte,
            meanRating_lt: this.meanRating.lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch province day meanRating success 3api");
          });
        return;
      } else if (
        //59
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?subjectName_in=" +
              this.subjectname +
              "&educationName_in=" +
              this.educationlevel +
              "&experienceName_in=" +
              this.experience
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch subjectname educationlevel experience success 3api"
            );
          });
        return;
      } else if (
        //60
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?subjectName_in=" +
              this.subjectname +
              "&educationName_in=" +
              this.educationlevel +
              "&timetostart_in=" +
              this.timetostart
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch subjectname educationlevel timetostart success 3api"
            );
          });
        return;
      } else if (
        //61
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?subjectName_in=" +
              this.subjectname +
              "&educationName_in=" +
              this.educationlevel +
              "&timetoend_in=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch subjectname educationlevel timetoend success 3api"
            );
          });
        return;
      } else if (
        //62
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            subjectName: this.subjectname,
            educationName: this.educationlevel,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel day success 3api"
            );
          });
        return;
      } else if (
        //63
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?subjectName_in=" +
              this.subjectname +
              "&educationName_in=" +
              this.educationlevel +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch subjectname educationlevel meanRating success 3api"
            );
          });
        return;
      } else if (
        //64
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?subjectName_in=" +
              this.subjectname +
              "&experienceName_in=" +
              this.experience +
              "&timetostart_in=" +
              this.timetostart
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch subjectname experience timetostart success 3api"
            );
          });
        return;
      } else if (
        //65
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?subjectName_in=" +
              this.subjectname +
              "&experienceName_in=" +
              this.experience +
              "&timetoend_in=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch subjectname experience timetoend success 3api"
            );
          });
        return;
      } else if (
        //66
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            subjectName: this.subjectname,
            experienceName: this.experience,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch subjectname experience day success 3api");
          });
        return;
      } else if (
        //67
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?subjectName_in=" +
              this.subjectname +
              "&experienceName_in=" +
              this.experience +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch subjectname experience meanRating success 3api"
            );
          });
        return;
      } else if (
        //68
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?subjectName_in=" +
              this.subjectname +
              "&timetostart_in=" +
              this.timetostart +
              "&timetoend_in=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch subjectname timetostart timetoend success 3api"
            );
          });
        return;
      } else if (
        //69
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            subjectName: this.subjectname,
            timetostart: this.timetostart,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname timetostart day success 3api"
            );
          });
        return;
      } else if (
        //70
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?subjectName_in=" +
              this.subjectname +
              "&timetostart_in=" +
              this.timetostart +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch subjectname timetostart timetoend success 3api"
            );
          });
        return;
      } else if (
        //71
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            subjectName: this.subjectname,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch subjectname timetoend day success 3api");
          });
        return;
      } else if (
        //72
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?subjectName_in=" +
              this.subjectname +
              "&timetoend _in=" +
              this.timetoend +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch subjectname timetoend meanRating success 3api"
            );
          });
        return;
      } else if (
        //73
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            subjectName: this.subjectname,
            meanRating_gte: this.meanRating.gte,
            meanRating_lt: this.meanRating.lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch subjectname day meanRating success 3api");
          });
        return;
      } else if (
        //74
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?educationName_in=" +
              this.educationlevel +
              "&experienceName_in=" +
              this.experience +
              "&timetostart_in=" +
              this.timetostart
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch educationlevel experience timetostart success 3api"
            );
          });
        return;
      } else if (
        //75
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?educationName_in=" +
              this.educationlevel +
              "&experienceName_in=" +
              this.experience +
              "&timetoend_in=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch educationlevel experience timetoend success 3api"
            );
          });
        return;
      } else if (
        //76
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            educationName: this.educationlevel,
            experienceName: this.experience,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel experience day success 3api"
            );
          });
        return;
      } else if (
        //77
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?educationName_in=" +
              this.educationlevel +
              "&experienceName_in=" +
              this.experience +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch educationlevel experience meanRating success 3api"
            );
          });
        return;
      } else if (
        //78
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?educationName_in=" +
              this.educationlevel +
              "&timetostart_in=" +
              this.timetostart +
              "&timetoend_in=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch educationlevel timetostart timetoend success 3api"
            );
          });
        return;
      } else if (
        //79
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel timetostart day success 3api"
            );
          });
        return;
      } else if (
        //80
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?educationName_in=" +
              this.educationlevel +
              "&timetostart_in=" +
              this.timetostart +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch educationlevel timetostart meanRating success 3api"
            );
          });
        return;
      } else if (
        //81
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            educationName: this.educationlevel,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel timetoend day success 3api"
            );
          });
        return;
      } else if (
        //82
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?educationName_in=" +
              this.educationlevel +
              "&timetoend_in=" +
              this.timetoend +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch educationlevel timetoend meanRating success 3api"
            );
          });
        return;
      } else if (
        //83
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            educationName: this.educationlevel,
            meanRating_gte: this.meanRating.gte,
            meanRating_lt: this.meanRating.lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel day meanRating success 3api"
            );
          });
        return;
      } else if (
        //84
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?experienceName_in=" +
              this.experience +
              "&timetostart_in=" +
              this.timetostart +
              "&timetoend_in=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch experience timetostart timetoend success 3api"
            );
          });
        return;
      } else if (
        //85
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            experienceName: this.experience,
            timetostart: this.timetostart,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch experience timetostart day success 3api");
          });
        return;
      } else if (
        //86
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?experienceName_in=" +
              this.experience +
              "&timetostart_in=" +
              this.timetostart +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch experience timetostart meanRating success 3api"
            );
          });
        return;
      } else if (
        //87
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            experienceName: this.experience,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch experience timetoend day success 3api");
          });
        return;
      } else if (
        //88
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?experienceName_in=" +
              this.experience +
              "&timetoend_in=" +
              this.timetoend +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch experience timetoend meanRating success 3api"
            );
          });
        return;
      } else if (
        //89
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            experienceName: this.experience,
            meanRating_gte: this.meanRating.gte,
            meanRating_lt: this.meanRating.lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch experience day meanRating success 3api");
          });
        return;
      } else if (
        //90
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch timetostart timetoend day success 3api");
          });
        return;
      } else if (
        //91
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?timetostart_in=" +
              this.timetostart +
              "&timetoend_in=" +
              this.timetoend +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch timetostart timetoend meanRating success 3api"
            );
          });
        return;
      } else if (
        //92
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            timetostart: this.timetostart,
            meanRating_gte: this.meanRating.gte,
            meanRating_lt: this.meanRating.lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch timetostart day meanRating success 3api");
          });
        return;
      } else if (
        //93
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating.gte,
            meanRating_lt: this.meanRating.lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log("get onsearch timetoend day meanRating success 3api");
          });
        return;
      } else if (
        //94
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&educationName_in=" +
              this.educationlevel +
              "&experienceName_in=" +
              this.experience
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname educationlevel experience success 4api"
            );
          });
        return;
      } else if (
        //95
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&educationName_in=" +
              this.educationlevel +
              "&timetostart_in=" +
              this.timetostart
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname educationlevel timetostart success 4api"
            );
          });
        return;
      } else if (
        //96
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&educationName_in=" +
              this.educationlevel +
              "&timetoend_in=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname educationlevel timetoend success 4api"
            );
          });
        return;
      } else if (
        //97
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: this.educationlevel,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel day success 4api"
            );
          });
        return;
      } else if (
        //98
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&educationName_in=" +
              this.educationlevel +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname educationlevel meanRating success 4api"
            );
          });
        return;
      } else if (
        //99
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&experienceName_in=" +
              this.experience +
              "&timetostart_in=" +
              this.timetostart
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname experience timetostart success 4api"
            );
          });
        return;
      } else if (
        //100
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&experienceName_in=" +
              this.experience +
              "&timetoend_in=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname experience timetoend success 4api"
            );
          });
        return;
      } else if (
        //101
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            provinceName: this.province,
            subjectName: this.subjectname,
            experienceName: this.experience,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname experience day success 4api"
            );
          });
        return;
      } else if (
        //102
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&experienceName_in=" +
              this.experience +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname experience meanRating success 4api"
            );
          });
        return;
      } else if (
        //103
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&timetostart_in=" +
              this.timetostart +
              "&timetoend_in=" +
              this.timetoend
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname timetostart timetoend success 4api"
            );
          });
        return;
      } else if (
        //104
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            provinceName: this.province,
            subjectName: this.subjectname,
            timetostart: this.timetostart,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname timetostart day success 4api"
            );
          });
        return;
      } else if (
        //105
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&timetostart_in=" +
              this.timetostart +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname timetostart meanRating success 4api"
            );
          });
        return;
      } else if (
        //106
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            provinceName: this.province,
            subjectName: this.subjectname,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname timetoend day success 4api"
            );
          });
        return;
      } else if (
        //107
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        axios
          .get(
            "http://localhost:1337/announcementposts?provinceName_in=" +
              this.province +
              "&subjectName_in=" +
              this.subjectname +
              "&timetoend_in=" +
              this.timetoend +
              "&_where[meanRating_gte]=" +
              this.meanRating.gte +
              "&[meanRating_lt]=" +
              this.meanRating.lt
          )
          .then((response) => {
            this.listsearchs = response.data;
            console.log(
              "get onsearch province subjectname timetoend meanRating success 4api"
            );
          });
        return;
      } else if (
        //108
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            day: this.value,
            provinceName: this.province,
            subjectName: this.subjectname,
            meanRating_gte: this.meanRating.gte,
            meanRating_lt: this.meanRating.lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname day meanRating success 4api"
            );
          });
        return;
      } else if (
        //109
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience timetostart success 4api"
            );
          });
        return;
      } else if (
        //110
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience timetoend success 4api"
            );
          });
        return;
      } else if (
        //111
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            experienceName: this.experience,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience day success 4api"
            );
          });
        return;
      } else if (
        //112
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            experienceName: this.experience,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience meanRating success 4api"
            );
          });
        return;
      } else if (
        //113
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel timetostart timetoend success 4api"
            );
          });
        return;
      } else if (
        //114
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel timetostart day success 4api"
            );
          });
        return;
      } else if (
        //115
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel timetostart meanRating success 4api"
            );
          });
        return;
      } else if (
        //116
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel timetoend day success 4api"
            );
          });
        return;
      } else if (
        //117
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel timetoend meanRating success 4api"
            );
          });
        return;
      } else if (
        //118
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel day meanRating success 4api"
            );
          });
        return;
      } else if (
        //119
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province experience timetostart timetoend success 4api"
            );
          });
        return;
      } else if (
        //120
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            experienceName: this.experience,
            timetostart: this.timetostart,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province experience timetostart day success 4api"
            );
          });
        return;
      } else if (
        //121
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            experienceName: this.experience,
            timetostart: this.timetostart,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province experience timetostart meanRating success 4api"
            );
          });
        return;
      } else if (
        //122
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            experienceName: this.experience,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province experience timetoend day success 4api"
            );
          });
        return;
      } else if (
        //123
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            experienceName: this.experience,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province experience timetoend meanRating success 4api"
            );
          });
        return;
      } else if (
        //124
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            experienceName: this.experience,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province experience day meanRating success 4api"
            );
          });
        return;
      } else if (
        //125
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province timetostart timetoend day success 4api"
            );
          });
        return;
      } else if (
        //126
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province timetostart timetoend meanRating success 4api"
            );
          });
        return;
      } else if (
        //127
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            timetostart: this.timetostart,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province timetostart day meanRating success 4api"
            );
          });
        return;
      } else if (
        //128
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province timetoend day meanRating success 4api"
            );
          });
        return;
      } else if (
        //129
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience timetostart success 4api"
            );
          });
        return;
      } else if (
        //130
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience timetoend success 4api"
            );
          });
        return;
      } else if (
        //131
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience day success 4api"
            );
          });
        return;
      } else if (
        //132
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience meanRating success 4api"
            );
          });
        return;
      } else if (
        //133
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel timetostart timetoend success 4api"
            );
          });
        return;
      } else if (
        //134
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel timetostart day success 4api"
            );
          });
        return;
      } else if (
        //135
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel timetostart meanRating success 4api"
            );
          });
        return;
      } else if (
        //136
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel timetoend day success 4api"
            );
          });
        return;
      } else if (
        //137
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel timetoend meanRating success 4api"
            );
          });
        return;
      } else if (
        //138
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel day meanRating success 4api"
            );
          });
        return;
      } else if (
        //139
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname experience timetostart timetoend success 4api"
            );
          });
        return;
      } else if (
        //140
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetostart: this.timetostart,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname experience timetostart day success 4api"
            );
          });
        return;
      } else if (
        //141
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetostart: this.timetostart,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname experience timetostart meanRating success 4api"
            );
          });
        return;
      } else if (
        //142
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname experience timetoend day success 4api"
            );
          });
        return;
      } else if (
        //143
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname experience timetoend meanRating success 4api"
            );
          });
        return;
      } else if (
        //144
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            experienceName: this.experience,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname experience day meanRating success 4api"
            );
          });
        return;
      } else if (
        //145
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname timetostart timetoend day success 4api"
            );
          });
        return;
      } else if (
        //146
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname timetostart timetoend meanRating success 4api"
            );
          });
        return;
      } else if (
        //147
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            timetostart: this.timetostart,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname timetostart day meanRating success 4api"
            );
          });
        return;
      } else if (
        //148
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname timetoend day meanRating success 4api"
            );
          });
        return;
      } else if (
        //149
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel experience timetostart timetoend success 4api"
            );
          });
        return;
      } else if (
        //150
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel experience timetostart day success 4api"
            );
          });
        return;
      } else if (
        //151
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel experience timetostart meanRating success 4api"
            );
          });
        return;
      } else if (
        //152
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel experience timetoend day success 4api"
            );
          });
        return;
      } else if (
        //153
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel experience timetoend meanRating success 4api"
            );
          });
        return;
      } else if (
        //154
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            experienceName: this.experience,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel experience day meanRating success 4api"
            );
          });
        return;
      } else if (
        //155
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel timetostart timetoend day success 4api"
            );
          });
        return;
      } else if (
        //156
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel timetostart timetoend meanRating success 4api"
            );
          });
        return;
      } else if (
        //157
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel timetostart day meanRating success 4api"
            );
          });
        return;
      } else if (
        //158
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel timetoend day meanRating success 4api"
            );
          });
        return;
      } else if (
        //159
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch experience timetostart timetoend day success 4api"
            );
          });
        return;
      } else if (
        //160
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch experience timetostart timetoend meanRating success 4api"
            );
          });
        return;
      } else if (
        //161
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            experienceName: this.experience,
            timetostart: this.timetostart,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch experience timetostart day meanRating success 4api"
            );
          });
        return;
      } else if (
        //162
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            experienceName: this.experience,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch experience timetoend day meanRating success 4api"
            );
          });
        return;
      } else if (
        //163
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch timetostart timetoend day meanRating success 4api"
            );
          });
        return;
      } else if (
        //164
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience timetostart success 5api"
            );
          });
        return;
      } else if (
        //165
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience timetostart success 5api"
            );
          });
        return;
      } else if (
        //166
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience timetoend success 5api"
            );
          });
        return;
      } else if (
        //167
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience day success 5api"
            );
          });
        return;
      } else if (
        //168
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience meanRating success 5api"
            );
          });
        return;
      } else if (
        //169
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel timetostart timetoend success 5api"
            );
          });
        return;
      } else if (
        //170
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel timetostart day success 5api"
            );
          });
        return;
      } else if (
        //171
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel timetostart meanRating success 5api"
            );
          });
        return;
      } else if (
        //172
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel timetoend day success 5api"
            );
          });
        return;
      } else if (
        //173
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel timetoend meanRating success 5api"
            );
          });
        return;
      } else if (
        //174
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel day meanRating success 5api"
            );
          });
        return;
      } else if (
        //175
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname experience timetostart timetoend success 5api"
            );
          });
        return;
      } else if (
        //176
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetostart: this.timetostart,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname experience timetostart day success 5api"
            );
          });
        return;
      } else if (
        //177
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetostart: this.timetostart,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname experience timetostart meanRating success 5api"
            );
          });
        return;
      } else if (
        //178
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname experience timetoend day success 5api"
            );
          });
        return;
      } else if (
        //179
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname experience timetoend meanRating success 5api"
            );
          });
        return;
      } else if (
        //180
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            experienceName: this.experience,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname experience day meanRating success 5api"
            );
          });
        return;
      } else if (
        //181
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname timetostart timetoend day success 5api"
            );
          });
        return;
      } else if (
        //182
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname timetostart timetoend meanRating success 5api"
            );
          });
        return;
      } else if (
        //183
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            timetostart: this.timetostart,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname timetostart day meanRating success 5api"
            );
          });
        return;
      } else if (
        //184
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname timetoend day meanRating success 5api"
            );
          });
        return;
      } else if (
        //185
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience timetostart timetoend success 5api"
            );
          });
        return;
      } else if (
        //186
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience timetostart day success 5api"
            );
          });
        return;
      } else if (
        //187
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience timetostart meanRating success 5api"
            );
          });
        return;
      } else if (
        //188
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience timetoend day success 5api"
            );
          });
        return;
      } else if (
        //189
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience timetoend meanRating success 5api"
            );
          });
        return;
      } else if (
        //190
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            experienceName: this.experience,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience day meanRating success 5api"
            );
          });
        return;
      } else if (
        //191
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel timetostart timetoend day success 5api"
            );
          });
        return;
      } else if (
        //192
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel timetostart timetoend meanRating success 5api"
            );
          });
        return;
      } else if (
        //193
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel timetostart day meanRating success 5api"
            );
          });
        return;
      } else if (
        //194
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel timetoend day meanRating success 5api"
            );
          });
        return;
      } else if (
        //195
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province experience timetostart timetoend day success 5api"
            );
          });
        return;
      } else if (
        //196
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province experience timetostart timetoend meanRating success 5api"
            );
          });
        return;
      } else if (
        //197
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            experienceName: this.experience,
            timetostart: this.timetostart,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province experience timetostart day meanRating success 5api"
            );
          });
        return;
      } else if (
        //198
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            experienceName: this.experience,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province experience timetoend day meanRating success 5api"
            );
          });
        return;
      } else if (
        //199
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province timetostart timetoend day meanRating success 5api"
            );
          });
        return;
      } else if (
        //200
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience timetostart timetoend success 5api"
            );
          });
        return;
      } else if (
        //201
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience timetostart day success 5api"
            );
          });
        return;
      } else if (
        //202
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience timetostart meanRating success 5api"
            );
          });
        return;
      } else if (
        //203
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience timetoend day success 5api"
            );
          });
        return;
      } else if (
        //204
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience timetoend meanRating success 5api"
            );
          });
        return;
      } else if (
        //205
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            experienceName: this.experience,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience day meanRating success 5api"
            );
          });
        return;
      } else if (
        //206
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel timetostart timetoend day success 5api"
            );
          });
        return;
      } else if (
        //207
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel timetostart timetoend meanRating success 5api"
            );
          });
        return;
      } else if (
        //208
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel timetostart day meanRating success 5api"
            );
          });
        return;
      } else if (
        //209
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName: this.educationlevel,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel timetoend  day meanRating success 5api"
            );
          });
        return;
      } else if (
        //210
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname experience timetostart timetoend  day success 5api"
            );
          });
        return;
      } else if (
        //211
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname experience timetostart timetoend meanRating success 5api"
            );
          });
        return;
      } else if (
        //212
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetostart: this.timetostart,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname experience timetostart day meanRating success 5api"
            );
          });
        return;
      } else if (
        //213
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname experience timetoend day meanRating success 5api"
            );
          });
        return;
      } else if (
        //214
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname timetostart timetoend day meanRating success 5api"
            );
          });
        return;
      } else if (
        //215
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel experience timetostart timetoend day success 5api"
            );
          });
        return;
      } else if (
        //216
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel experience timetostart timetoend meanRating success 5api"
            );
          });
        return;
      } else if (
        //217
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel experience timetostart day meanRating success 5api"
            );
          });
        return;
      } else if (
        //218
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            experienceName: this.experience,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel experience timetoend day meanRating success 5api"
            );
          });
        return;
      } else if (
        //219
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel timetostart timetoend day meanRating success 5api"
            );
          });
        return;
      } else if (
        //220
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch experience timetostart timetoend day meanRating success 5api"
            );
          });
        return;
      } else if (
        //221
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            timetoend: this.timetoend,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience timetostart timetoend success 6api"
            );
          });
        return;
      } else if (
        //222
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience timetostart day success 6api"
            );
          });
        return;
      } else if (
        //223
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: educationlevel,
            experienceName: this.experience,
            timetostart: this.timetostart,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience timetostart meanRating success 6api"
            );
          });
        return;
      } else if (
        //224
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: educationlevel,
            experienceName: this.experience,
            timetoend: this.timetoend,
            day: this.value,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience timetoend day success 6api"
            );
          });
        return;
      } else if (
        //225
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: educationlevel,
            experienceName: this.experience,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience timetoend meanRating success 6api"
            );
          });
        return;
      } else if (
        //226
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: educationlevel,
            experienceName: this.experience,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //227
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: educationlevel,
            timetostart: this.timetostart,
            timetoend:this.timetoend,
            day: this.value
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel timetostart timetoend day success 6api"
            );
          });
        return;
      }
       else if (
        //228
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: educationlevel,
            timetostart: this.timetostart,
            timetoend:this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel timetostart timetoend meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //229
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: educationlevel,
            timetostart: this.timetostart,
            day:this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel timetostart day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //230
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            educationName: educationlevel,
            timetoend: this.timetoend,
            day:this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel timetoend day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //231
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetostart:this.timetostart,
            timetoend: this.timetoend,
            day:this.value
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname experience timetostart timetoend day success 6api"
            );
          });
        return;
      }
       else if (
        //232
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetostart:this.timetostart,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname experience timetostart timetoend meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //233
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetostart:this.timetostart,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname experience timetostart day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //234
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            experienceName: this.experience,
            timetoend:this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname experience timetoend day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //235
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            subjectName: this.subjectname,
            timetostart: this.timetostart,
            timetoend:this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname timetostart timetoend day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //236
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            experienceName:this.experience,
            timetostart: this.timetostart,
            timetoend:this.timetoend,
            day: this.value
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience timetostart timetoend day success 6api"
            );
          });
        return;
      }
      else if (
        //237
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            experienceName:this.experience,
            timetostart: this.timetostart,
            timetoend:this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience timetostart timetoend meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //238
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            experienceName:this.experience,
            timetostart: this.timetostart,
            day:this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience timetostart day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //239
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            experienceName:this.experience,
            timetoend: this.timetoend,
            day:this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience timetoend day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //240
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            educationName: this.educationlevel,
            timetostart:this.timetostart,
            timetoend: this.timetoend,
            day:this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel timetostart timetoend day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //241
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName: this.province,
            experienceName: this.experience,
            timetostart:this.timetostart,
            timetoend: this.timetoend,
            day:this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province experience timetostart timetoend day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //242
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName:this.educationlevel,
            experienceName: this.experience,
            timetostart:this.timetostart,
            timetoend: this.timetoend,
            day:this.value
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience timetostart timetoend day success 6api"
            );
          });
        return;
      }
      else if (
        //243
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName:this.educationlevel,
            experienceName: this.experience,
            timetostart:this.timetostart,
            timetoend: this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience timetostart timetoend meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //244
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName:this.educationlevel,
            experienceName: this.experience,
            timetostart:this.timetostart,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience timetostart day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //245
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName:this.educationlevel,
            experienceName: this.experience,
            timetoend:this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience timetoend day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //246
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            educationName:this.educationlevel,
            timetostart: this.timetostart,
            timetoend:this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel timetostart timetoend day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //247
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName: this.subjectname,
            experienceName:this.experience,
            timetostart: this.timetostart,
            timetoend:this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname experience timetostart timetoend day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //248
        searchedprovicetype == null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            educationName: this.educationlevel,
            experienceName:this.experience,
            timetostart: this.timetostart,
            timetoend:this.timetoend,
            day: this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch educationlevel experience timetostart timetoend day meanRating success 6api"
            );
          });
        return;
      }
      else if (
        //249
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating == null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName:this.province,
            subjectName:this.subjectname,
            educationName: this.educationlevel,
            experienceName:this.experience,
            timetostart: this.timetostart,
            timetoend:this.timetoend,
            day: this.value
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience timetostart timetoend day success 7api"
            );
          });
        return;
      }
      else if (
        //250
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length == true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName:this.province,
            subjectName:this.subjectname,
            educationName: this.educationlevel,
            experienceName:this.experience,
            timetostart: this.timetostart,
            timetoend:this.timetoend,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience timetostart timetoend meanRating success 7api"
            );
          });
        return;
      }
      else if (
        //251
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend == null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName:this.province,
            subjectName:this.subjectname,
            educationName: this.educationlevel,
            experienceName:this.experience,
            timetostart: this.timetostart,
            day:this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience timetostart day meanRating success 7api"
            );
          });
        return;
      }
      else if (
        //252
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart == null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName:this.province,
            subjectName:this.subjectname,
            educationName: this.educationlevel,
            experienceName:this.experience,
            timetoend: this.timetoend,
            day:this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience timetoend day meanRating success 7api"
            );
          });
        return;
      }
      else if (
        //253
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype == null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName:this.province,
            subjectName:this.subjectname,
            educationName: this.educationlevel,
            timetostart:this.timetostart,
            timetoend: this.timetoend,
            day:this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel timetostart timetoend day meanRating success 7api"
            );
          });
        return;
      }
      else if (
        //254
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype == null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName:this.province,
            subjectName:this.subjectname,
            experienceName: this.experience,
            timetostart:this.timetostart,
            timetoend: this.timetoend,
            day:this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname experience timetostart timetoend day meanRating success 7api"
            );
          });
        return;
      }
      else if (
        //255
        searchedprovicetype != null &&
        searchedsubjecttype == null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName:this.province,
            educationName:this.educationlevel,
            experienceName: this.experience,
            timetostart:this.timetostart,
            timetoend: this.timetoend,
            day:this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province educationlevel experience timetostart timetoend day meanRating success 7api"
            );
          });
        return;
      }
      else if (
        //256
        searchedprovicetype == null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            subjectName:this.subjectname,
            educationName:this.educationlevel,
            experienceName: this.experience,
            timetostart:this.timetostart,
            timetoend: this.timetoend,
            day:this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch subjectname educationlevel experience timetostart timetoend day meanRating success 7api"
            );
          });
        return;
      }
      else if (
        //257
        searchedprovicetype != null &&
        searchedsubjecttype != null &&
        searchededucationleveltype != null &&
        searchedexperiencetype != null &&
        searchtimetostart != null &&
        searchtimetoend != null &&
        !searchvalue?.length != true && // ในอาเรย์เป็นnullไหม ถ้าเป็นnull  (!searchvalue?.length ) จะเป็นtrue
        searchmeanRating != null
      ) {
        strapi
          .getEntries("announcementposts", {
            provinceName:this.province,
            subjectName:this.subjectname,
            educationName:this.educationlevel,
            experienceName: this.experience,
            timetostart:this.timetostart,
            timetoend: this.timetoend,
            day:this.value,
            meanRating_gte: this.meanRating_gte,
            meanRating_lt: this.meanRating_lt,
          })
          .then((res) => {
            this.listsearchs = res; // res มันคือ data แล้ว
            console.log(
              "get onsearch province subjectname educationlevel experience timetostart timetoend day meanRating success 8api"
            );
          });
        return;
      }
    },
  },
};
</script>
