<template>
  <div>
    <Header />
    <nuxt class="router content-wrapper" />
    <Footer class="footer" />
  </div>
</template>
<script>
import Header from "~/components/Header.vue";
import Footer from "~/components/Footer.vue";
export default {
  components: {
    Header,
    Footer
  }
};
</script>


<style>
.router{
   text-decoration: none;
  color: black;
  height:100%;
    min-height: 100%;

    position:relative;
}
.footer{
    width:100%;
    bottom:0px;
}
</style>
