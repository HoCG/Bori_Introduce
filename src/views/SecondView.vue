<template>
  <div class="dog-box">
    <BaDukEi></BaDukEi>
    <BaDukEi></BaDukEi>
    <BaDukEi></BaDukEi>
    <BaDukEi></BaDukEi>
  </div>
</template>
<script>
import BaDukEi from "@/components/BaDukEi.vue";
//import WelshCorgi from "@/components/WelshCorgi.vue"
export default {
  components: {
    BaDukEi
  }
}
</script>
<style>
  .dog-box  {
    display: flex;
  }
</style>