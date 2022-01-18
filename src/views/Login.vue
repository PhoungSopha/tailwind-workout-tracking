<template>
  <div class="max-w-screen-sm px-4 py-10 mx-auto">
    <!-- Error handling -->
    <div v-if="errMsg" class="p-4 mb-4 rounded-md bg-light-grey">
      <p class="text-red-500">{{ errMsg }}</p>
    </div>

    <!-- Login form -->
    <form
      @submit.prevent="login"
      class="flex flex-col p-8 bg-opacity-50 rounded-md shadow-lg bg-light-grey"
    >
      <h1 class="mb-4 text-2xl text-at-light-green">Log in</h1>

      <div class="flex flex-col mb-3">
        <label for="Email" class="mb-3 text-sm font-bold text-at-light-green"
          >Email</label
        >
        <input type="text" class="input" id="email" v-model="email" required />
      </div>

      <div class="flex flex-col mb-3">
        <label for="Password" class="mb-3 text-sm font-bold text-at-light-green"
          >Password</label
        >
        <input
          type="password"
          class="input"
          id="password"
          v-model="password"
          required
        />
      </div>

      <button type="submit" class="submit-button">Log in</button>

      <router-link
        class="mt-6 text-sm font-medium text-center text-gray-500"
        :to="{ name: 'Register' }"
        >Not yet have an account?<span class="text-at-light-green">
          Register</span
        ></router-link
      >
    </form>
  </div>
</template>

<script>
import { ref } from "vue";
import { supabase } from "../supabase/init";
import { useRouter } from "vue-router";
export default {
  name: "register",
  setup() {
    const email = ref(null);
    const password = ref(null);
    const errMsg = ref(null);
    const router = useRouter();
    // Create data / vars

    // Login function
    const login = async () => {
      try {
        const { error } = await supabase.auth.signIn({
          email: email.value,
          password: password.value,
        });
        if (error) throw error;
        router.push({ name: "Home" });
      } catch (error) {
        errMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errMsg.value = null;
        }, 5000);
      }
    };

    return { email, password, errMsg, login };
  },
};
</script>
<style scoped>
.submit-button {
  @apply self-start px-6 py-2 mt-6 text-sm text-white 
  duration-200 border-2 border-transparent border-solid rounded-sm 
  bg-at-light-green hover:border-white hover:bg-white hover:text-at-light-green hover:border-at-light-green;
}

.input {
  @apply p-2 text-gray-500 focus:outline-none focus:ring-2 focus:ring-at-light-green;
}
</style>
