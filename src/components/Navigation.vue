<template>
  <header class="text-white bg-at-light-green">
    <nav
      class="container flex items-center gap-4 px-4 py-5 col sm:flex-row"
    >
      <!-- Logo -->
      <div class="flex items-center gap-x-4">
        <img
          class="w-14"
          src="../assets/images/dumbbell-light.png"
          alt=""
        />
        <p class="font-medium font-bold">Active Tracker</p>
      </div>
      <!-- menu -->
      <ul class="flex justify-end flex-1 gap-x-10">
        <router-link class="cursor-pointer" :to="{ name: 'Home' }"
          >Home</router-link
        >
        <router-link
          v-if="users"
          class="cursor-pointer hover:border-b-2 hover:border-white"
          :to="{ name: 'Create' }"
          >Create</router-link
        >
        <router-link
          v-if="!users"
          class="cursor-pointer"
          :to="{ name: 'Login' }"
          >Log in</router-link
        >
        <li v-if="users" @click="logout" class="cursor-pointer">
          Log out
        </li>
      </ul>
    </nav>
  </header>
</template>

<script>
import store from "../store/index";
import { supabase } from "../supabase/init";
import { computed } from "vue";
import { useRouter } from "vue-router";
export default {
  setup() {
    // Get user from store
    const users = computed(() => store.state.user);
    // Setup ref to router
    const router = useRouter();

    // Logout function
    const logout = async () => {
      await supabase.auth.signOut();
      router.push({ name: "Home" });
      console.log("you are logged out now");
    };

    return { logout, users };
  },
};
</script>
