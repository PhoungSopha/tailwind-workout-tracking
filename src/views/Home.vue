<template>
  <div v-if="dataLoaded" class="container px-4 mt-10">
    <!-- No Data -->
    <div
      v-if="data.length === 0"
      class="flex flex-col w-full text-center"
    >
      <h1 class="text-2xl">Look, have no record yet...</h1>
      <button
        class="w-40 px-4 py-2 mx-auto mt-6 text-sm text-white duration-200 border-2 border-transparent border-solid rounded-sm bg-at-light-green hover:border-green hover:bg-white hover:text-at-light-green hover:border-at-light-green"
      >
        <router-link :to="{ name: 'Create' }"
          >Create Workout</router-link
        >
      </button>
    </div>

    <!-- Data -->

    <div
      v-else
      class="grid grid-cols-1 gap-6 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4"
    >
      <router-link
        class="flex flex-col items-center p-8 shadow-md cursor-pointer bg-light-grey hover:shadow-lg"
        :to="{
          name: 'View-Workout',
          params: { workoutId: workouts.id },
        }"
        v-for="(workouts, index) in data"
        :key="index"
      >
        <!-- cardio image -->

        <img
          v-if="workouts.workoutType === 'cardio'"
          src="../assets/images/running-light-green.png"
          class="w-auto h-24 "
          alt=""
        />

        <!-- strength training -->

        <img
          v-if="workouts.workoutType === 'strength'"
          class="w-auto h-24 "
          src="../assets/images/dumbbell-light-green.png"
          alt=""
        />

        <p
          class="px-3 py-1 mt-6 text-xs text-white rounded-lg shadow-md bg-at-light-green"
        >
          {{ workouts.workoutType }}
        </p>

        <h1 class="mt-8 mb-2 text-xl text-center text-at-light-green">
          {{ workouts.workoutName }}
        </h1>
      </router-link>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { supabase } from "../supabase/init";
export default {
  name: "Home",
  components: {},
  setup() {
    // Create data / vars
    const data = ref([]);
    const dataLoaded = ref(null);

    // Get data
    const getData = async () => {
      try {
        const { data: workouts, error } = await supabase
          .from("workouts")
          .select("*");
        if (error) throw error;
        data.value = workouts;
        dataLoaded.value = true;
      } catch (error) {
        console.log(error.message);
      }
    };
    // Run data function
    getData();
    return { dataLoaded, getData, data };
  },
};
</script>
