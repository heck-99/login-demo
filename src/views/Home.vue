<template>
  <div>
    <h1>Welcome to the Air Quality Check!</h1>
    <p>You can find the air quality for your city and see if it meets your threshold for "good" air.</p>
    <air-check v-if="isAuthenticated"/>
    <p v-else>
        Please login so that your preferences can be saved.
    </p>
    
    
  </div>
</template>

<script>
import { useAuth0 } from '@auth0/auth0-vue';
import AirCheck from "../components/AirCheck.vue";
// import Hero from "../components/Hero.vue";
// import HomeContent from "../components/HomeContent.vue";

export default {
  name: "home-view",
  components: {
    AirCheck
    // Hero,
    // HomeContent,
  },
  setup(){
    const auth0 = useAuth0();
    return {
      isAuthenticated: auth0.isAuthenticated,
    }
  }
};
</script>

<style lang="css" scoped>
.next-steps .fa-link {
    margin-right: 5px;
}
</style>
