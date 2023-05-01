<template>
  <div class="container">
    <div class="row">
      <TableComponent />
    </div>
  </div>
  <span :class="showGoTop ? 'go-top-show' : ''" class="go-top" v-on:click="goTo()">
    <img src="up-arrow.svg" alt="" width="20">
  </span>
</template>

<script>
import TableComponent from "./components/Table.vue";

export default {
  name: "App",
  components: { TableComponent },
  data(){
    return{
      showGoTop : false
    }
  },
  mounted() {
    this.handleScroll()
  },
  methods:{
    handleScroll(){
      window.addEventListener('scroll', () => {
        if (scrollY > 0) {
          this.showGoTop = true
        }else{
          this.showGoTop = false
        }
      })
    },
    goTo(){
      window.scrollTo({ top: 0, behavior: 'smooth' });
    }
  }
  
};
</script>
