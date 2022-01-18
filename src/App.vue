<template>
  <div v-if="appReady" class="box-border min-h-full font-Poppins">
    <NavigationNew />
    <router-view />
  </div>
</template>

<script>
import NavigationNew from "./components/NavigationNew.vue";

import { ref } from "vue";
import { supabase } from "./supabase/init";
import store from "./store/index";
export default {
  components: { NavigationNew },
  setup() {
    // Create data / vars
    const appReady = ref(null);

    // Check to see if user is already logged in
    const user = supabase.auth.user();
    // If user does not exist, need to make app ready
    if (!user) {
      appReady.value = true;
    }
    // Runs when there is a auth state change
    // if user is logged in, this will fire
    supabase.auth.onAuthStateChange((_, session) => {
      console.log("You are logged in now");
      store.methods.setUser(session);
      appReady.value = true;
    });

    return { appReady };
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700&display=swap");
</style>
