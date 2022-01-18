<template>
  <div class="max-w-screen-md px-4 py-10 mx-auto relative">
    <!-- status or error msg -->
    <div v-if="statusMsg || errorMsg" class="p-4 mb-5 rounded-md bg-light-grey">
      <p class="text-at-light-green">{{ statusMsg }}</p>
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>
    <!-- <div class="absolute w- h-10 max-w-2xl bg-at-light-green p-2 flex items-center gap-x-3">
      <span></span>
      <p>{{ statueSuccess }}</p>
    </div> -->
    <!-- Create Exercise -->
    <div class="flex items-start p-8 rounded-md shadow-lg bg-light-grey">
      <!-- Form -->
      <form @submit.prevent="createWorkout" class="flex flex-col w-full gap-y-5">
        <h1 class="text-2xl text-at-light-green">Record Workout</h1>
        <!-- workout name -->
        <div class="flex flex-col">
          <label for="workout-name" class="mb-1 text-sm text-at-light-green">Workout Name</label>
          <input type="text" class="input" id="workout-name" v-model="workoutName" required />
        </div>
        <!-- workoutType here -->
        <div class="flex flex-col">
          <label for="workout-type" class="mb-1 text-sm text-at-light-green">Workout Type</label>
          <select @change="workoutChange" id="workout-type" class="p-2 text-gray-500 focus:outline-none" v-model="workoutType" required>
            <option value="selete-workout">Select Workout</option>
            <option value="strength">Strength Trainning</option>
            <option value="cardio">Cardio</option>
          </select>
        </div>

        <!-- Strength Training -->

        <div v-if="workoutType === 'strength'" class="flex flex-col gap-y-10 sm:gap-y-4">
          <div class="relative flex flex-col gap-x-6 gap-y-2 md:flex-row" v-for="(item, index) in exercises" :key="index">
            <div class="flex flex-col md:w-1/3">
              <label for="exercise-name" class="mb-1 text-sm text-at-light-green">
                Exercise
              </label>
              <input type="text" class="w-full p-2 text-gray-500 focus:outline-none" v-model="item.exercise" required />
            </div>

            <div class="flex flex-col flex-1">
              <label for="sets" class="mb-1 text-sm text-at-light-green">
                Sets
              </label>
              <input type="text" class="w-full p-2 text-gray-500 focus:outline-none" v-model="item.sets" required />
            </div>

            <div class="flex flex-col flex-1">
              <label for="reps" class="mb-1 text-sm text-at-light-green">
                Reps
              </label>
              <input type="text" class="w-full p-2 text-gray-500 focus:outline-none" v-model="item.reps" required />
            </div>

            <div class="flex flex-col flex-1">
              <label for="weight" class="mb-1 text-sm text-at-light-green">
                Weight (LB's)
              </label>
              <input type="text" class="w-full p-2 text-gray-500 focus:outline-none" v-model="item.weight" required />
            </div>

            <!-- <img
              src="../assets/images/trash-light-green.png"
              class="relative object-cover w-auto h-8 p-2 ml-2 bg-white rounded-full cursor-pointer -left-4 top-8"
              alt=""
            /> -->
            <div class="relative ml-5">
              <svg @click="deleteExercise(item.id)" xmlns="http://www.w3.org/2000/svg" class="absolute w-auto h-8 p-1.5 ml-5 text-green-500 bg-white rounded-md fill-current right-0 top-0 sm:top-7 cursor-pointer" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
              </svg>
            </div>
          </div>

          <button @click="addExercise" type="submit" class="submit-button">
            Add Exercise
          </button>
        </div>

        <!-- Cardio Here -->

        <div v-if="workoutType === 'cardio'" class="flex flex-col gap-y-10 sm:gap-y-4">
          <div class="relative flex flex-col gap-x-6 gap-y-2 md:flex-row" v-for="(item, index) in exercises" :key="index">
            <div class="flex flex-col md:w-1/3">
              <label for="cardio-type" class="mb-1 text-sm text-at-light-green">
                Type
              </label>
              <select id="cardio-type" class="w-full p-2 text-gray-500 focus:outline-none" v-model="item.cardioType">
                <option value="#">Select Type</option>
                <option value="run">Run</option>
                <option value="walk">Walk</option></select
              >
            </div>

            <div class="flex flex-col flex-1">
              <label for="distance" class="mb-1 text-sm text-at-light-green">
                Distance
              </label>
              <input type="text" class="w-full p-2 text-gray-500 focus:outline-none" v-model="item.distance" required />
            </div>

            <div class="flex flex-col flex-1">
              <label for="duration" class="mb-1 text-sm text-at-light-green">
                Duration
              </label>
              <input type="text" class="w-full p-2 text-gray-500 focus:outline-none" v-model="item.duration" required />
            </div>

            <div class="flex flex-col flex-1">
              <label for="pace" class="mb-1 text-sm text-at-light-green">
                Pace
              </label>
              <input type="text" class="w-full p-2 text-gray-500 focus:outline-none" v-model="item.pace" required />
            </div>

            <!-- <img
              src="../assets/images/trash-light-green.png"
              class="relative object-cover w-auto h-8 p-2 ml-2 bg-white rounded-full cursor-pointer -left-4 top-8"
              alt=""
            /> -->
            <div class="relative ml-5">
              <svg @click="deleteExercise(item.id)" xmlns="http://www.w3.org/2000/svg" class="absolute w-auto h-8 p-1.5 ml-5 text-green-500 bg-white rounded-md fill-current right-0 top-0 sm:top-7 cursor-pointer" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
              </svg>
            </div>
          </div>

          <button @click="addExercise" type="submit" class="submit-button">
            Add Exercise
          </button>
        </div>

        <button type="submit" class="submit-button">
          Record Workout
        </button>
      </form>
    </div>
  </div>
</template>

<script>
// import { supabase } from "../supabase/init";
import { ref } from "vue"
import { uid } from "uid"
import { supabase } from "../supabase/init"
export default {
  name: "create",
  setup() {
    // Create data
    const exercises = ref([])
    const workoutName = ref(null)
    const workoutType = ref("select-workout")
    const statusMsg = ref(null)
    const errorMsg = ref(null)
    // Add exercise
    const addExercise = () => {
      if (workoutType.value === "strength") {
        exercises.value.push({
          id: uid(),
          exercise: "",
          sets: "",
          reps: "",
          weight: "",
        })
        return
      }
      exercises.value.push({
        id: uid(),
        cardioType: "",
        distance: "",
        duration: "",
        pace: "",
      })
    }
    // Delete exercise

    const deleteExercise = (id) => {
      if (exercises.value.length > 1) {
        exercises.value = exercises.value.filter((exercise) => exercise.id !== id)
        return
      }
      errorMsg.value = "Error: can not remove, need at least one exercise. "
      setTimeout(() => {
        errorMsg.value = false
      }, 5000)
    }

    // Listens for chaging of workout type input

    const workoutChange = () => {
      exercises.value = []
      addExercise()
    }
    // Create workout

    const createWorkout = async () => {
      try {
        const { error } = await supabase.from("workouts").insert([
          {
            workoutName: workoutName.value,
            workoutType: workoutType.value,
            exercises: exercises.value,
          },
        ])
        if (error) throw error
        statusMsg.value = "Workout is created successfully."
        workoutName.value = null
        workoutType.value = "select-workout"
        exercises.value = []
        setTimeout(() => {
          statusMsg.value = false
        }, 5000)
      } catch (error) {
        setTimeout(() => {
          errorMsg.value = `Error: ${error.message}`
          errorMsg.value = false
        }, 5000)
      }
    }

    return {
      exercises,
      workoutName,
      workoutType,
      statusMsg,
      errorMsg,
      addExercise,
      deleteExercise,
      workoutChange,
      createWorkout,
    }
  },
}
</script>

<style scoped>
.input {
  @apply p-2 text-gray-500 focus:outline-none focus:ring-2 focus:ring-at-light-green;
}

.submit-button {
  @apply self-start px-6 py-2 mt-6 text-sm text-white 
  duration-200 border-2 border-transparent border-solid rounded-sm 
  bg-at-light-green hover:border-white hover:bg-white hover:text-at-light-green hover:border-at-light-green;
}
</style>
