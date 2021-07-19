<template>
  <div class="about">
     <personal-center></personal-center>
  </div>
</template>

<script>

  import PersonalCenter from "@/components/about/PersonalCenter.vue";

  export default {
    name: "about",
    data(){
      return{
      }
    },
    components: {
      PersonalCenter
    }
  };
</script>
