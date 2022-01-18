<template>
  <div
    class="items-center font-sans antialiased text-white bg-at-light-green"
    id="app"
  >
    <nav
      class="container flex flex-wrap items-center justify-between p-6 bg-at-light-green"
    >
      <div class="flex items-center mr-6 text-white flex-no-shrink ">
        <img
          class="w-14"
          src="../assets/images/dumbbell-light.png"
          alt=""
        />
        <span class="ml-3 font-semibold tracking-tight font-xl"
          >Active Tracker</span
        >
      </div>

      <div class="block sm:hidden">
        <svg
          @click="toggle"
          xmlns="http://www.w3.org/2000/svg"
          class="w-auto h-8 p-1 border border-white rounded-md cursor-pointer hover:fill-current hover:text-at-light-green hover:bg-white focus:outline-none"
          viewBox="0 0 20 20"
          fill="currentColor"
        >
          <path
            fill-rule="evenodd"
            d="M3 5a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 10a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 15a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z"
            clip-rule="evenodd"
          />
        </svg>
        <!-- <button
          @click="toggle"
          class="flex items-center px-3 py-2 text-xl text-white rounded hover:text-white hover:border-black"
        >
          <svg
            class="w-3 h-3 fill-current"
            viewBox="0 0 20 20"
            xmlns="http://www.w3.org/2000/svg"
          >
            <title>Menu</title>
            <path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z" />
          </svg>
        </button> -->
      </div>

      <div
        :class="open ? 'block' : 'hidden'"
        class="flex-grow w-full sm:flex sm:items-center sm:w-auto"
      >
        <div
          class="flex flex-col justify-end text-md sm:flex-grow gap-x-5 sm:flex-row"
        >
          <hr class="mt-5 bg-white" />
          <router-link
            @click="toggle"
            class="menu"
            :to="{ name: 'Home' }"
            >Home</router-link
          >

          <router-link
            v-if="user"
            class="menu"
            :to="{ name: 'Create' }"
            @click="toggle"
            >Create</router-link
          >
          <router-link
            @click="toggle"
            v-if="!user"
            class="menu"
            :to="{ name: 'Login' }"
            >Login</router-link
          >
          <p v-if="user" @click="logout" class="cursor-pointer menu">
            Logout
          </p>
        </div>
      </div>
    </nav>
  </div>
</template>
<script>
import { ref, computed } from "vue";
import { useRouter } from "vue-router";
import { supabase } from "../supabase/init";
import store from "../store/index";

export default {
  setup() {
    const router = useRouter();
    const user = computed(() => store.state.user);
    const open = ref(false);

    const logout = async () => {
      await supabase.auth.signOut();
      router.push({ name: "Home" });
    };

    const toggle = () => {
      open.value = !open.value;
    };

    return { logout, user, open, toggle };
  },
};
//   data() {
//     return {
//       open: false,
//     };
//   },

//   methods: {
//     toggle() {
//       this.open = !this.open;
//     },
//   },
// };
</script>

<style scoped>
.menu {
  @apply block mt-4 mr-4 no-underline sm:inline-block sm:mt-0 hover:text-gray-200;
}
</style>
