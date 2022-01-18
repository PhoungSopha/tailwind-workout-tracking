<template>
  <div class="max-w-screen-sm px-4 py-10 mx-auto">
    <!-- Error handling -->
    <div v-if="errMsg" class="p-4 mb-4 rounded-md bg-light-grey">
      <p class="text-red-500">{{ errMsg }}</p>
    </div>

    <!-- Register form -->
    <form
      @submit.prevent="register"
      class="flex flex-col p-8 bg-opacity-50 rounded-md shadow-lg bg-light-grey"
    >
      <h1 class="mb-4 text-2xl text-at-light-green">Register</h1>

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

      <div class="flex flex-col mb-3">
        <label
          for="confirmPassword"
          class="mb-3 text-sm font-bold text-at-light-green"
          >Confirm Password</label
        >
        <input
          type="password"
          class="input"
          id="confirmPassword"
          v-model="confirmPassword"
          required
        />
      </div>

      <button type="submit" class="submit-button">Register</button>

      <router-link
        class="mt-6 text-sm font-medium text-center text-gray-500"
        :to="{ name: 'Login' }"
        >Already have an account?<span class="text-at-light-green">
          Login</span
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
    // Create data / vars
    const email = ref(null);
    const password = ref(null);
    const confirmPassword = ref(null);
    const errMsg = ref(null);
    const router = useRouter();
    // Register function

    const register = async () => {
      if (password.value === confirmPassword.value) {
        try {
          const { error } = await supabase.auth.signUp({
            email: email.value,
            password: password.value,
          });
          if (error) throw error;
          router.push({ name: "Login" });
        } catch (error) {
          errMsg.value = error.message;
        }
        return;
      }
      errMsg.value = "Error: Password do not matched";
      setTimeout(() => {
        errMsg.value = null;
      }, 5000);
    };

    return { email, password, confirmPassword, errMsg, register };
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
