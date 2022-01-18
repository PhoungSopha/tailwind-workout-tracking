<template>
  <div class="max-w-screen-sm px-5 py-10 mx-auto ">
    <!-- App Message -->
    <div v-if="errorMsg || statusMsg" class="p-4 mb-5 rounded-md shadow-md bg-light-grey">
      <span class="flex items-center" v-if="statusMsg"
        ><svg xmlns="http://www.w3.org/2000/svg" class="w-auto fill-current h-7 text-at-light-green" viewBox="0 0 20 20" fill="currentColor">
          <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
        </svg>
        <p class="pl-2 text-at-light-green">{{ statusMsg }}</p></span
      >
      <span v-else class="flex items-center">
        <svg xmlns="http://www.w3.org/2000/svg" class="w-auto text-red-500 fill-current h-7" viewBox="0 0 20 20" fill="currentColor">
          <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z" clip-rule="evenodd" />
        </svg>
        <p class="pl-2 text-red-500">{{ errorMsg }}</p>
      </span>
    </div>

    <!-- get info of exercise  -->
    <div v-if="dataLoaded">
      <!-- general workout info -->
      <div class="relative flex flex-col items-center p-8 rounded-md shadow-md bg-light-grey">
        <!-- update & delete icon -->
        <div v-if="user" class="absolute flex flex-row top-2 left-2 gap-x-2">
          <!-- update workout model -->
          <div class="relative ml-8">
            <svg @click="editMode" xmlns="http://www.w3.org/2000/svg" class="absolute w-auto h-8 p-1.5 text-white  bg-at-light-green rounded-full fill-current right-0 top-0 cursor-pointer hover:hover:shadow-md" viewBox="0 0 20 20" fill="currentColor">
              <path d="M13.586 3.586a2 2 0 112.828 2.828l-.793.793-2.828-2.828.793-.793zM11.379 5.793L3 14.172V17h2.828l8.38-8.379-2.83-2.828z" />
            </svg>
          </div>
          <!-- delete workout -->
          <div v-if="!edit" class="relative ml-8">
            <svg @click="showModal" xmlns="http://www.w3.org/2000/svg" class="absolute w-auto h-8 p-1.5 text-white  bg-at-light-green rounded-full fill-current right-0 top-0 cursor-pointer hover:hover:shadow-md" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
            </svg>
          </div>
        </div>

        <img v-if="data.workoutType === 'cardio'" class="w-auto h-24" src="../assets/images/running-light-green.png" alt="" />

        <img v-else class="w-auto h-24" src="../assets/images/dumbbell-light-green.png" alt="" />

        <span class="mt-6 py-1.5 px-5 text-xs text-white bg-at-light-green rounded-lg shadow-md">{{ data.workoutType }}</span>

        <div class="w-full mt-6">
          <input v-if="edit" type="text" class="w-full p-2 text-gray-500 focus:outline-none" v-model="data.workoutName" />
          <h1 v-else class="text-2xl text-center text-at-light-green">
            {{ data.workoutName }}
          </h1>
        </div>
      </div>

      <!-- Exercise Info -->
      <div class="flex flex-col items-center p-8 mt-10 rounded-md shadow-md bg-light-grey">
        <!-- Streng Trainning -->
        <div v-if="data.workoutType === 'strength'" class="flex flex-col w-full gap-y-4">
          <div class="relative flex felx-col gap-x-4 gap-y-6 sm:flex-row " v-for="(item, index) in data.exercises" :key="index">
            <div class="flex flex-col flex-2 md:w-1/3">
              <label for="exercise-name" class="mb-1 text-sm text-at-light-green">Exercise</label>
              <input v-if="edit" id="exercise-name" type="text" class="w-full text-gray-500 focus:outline-none" v-model="item.exercise" />
              <p v-else>{{ item.exercise }}</p>
            </div>

            <div class="flex flex-col flex-1 ">
              <label for="sets" class="mb-1 text-sm text-at-light-green">Sets</label>
              <input v-if="edit" id="sets" type="text" class="w-full text-gray-500 focus:outline-none" v-model="item.sets" />
              <p v-else>{{ item.sets }}</p>
            </div>

            <div class="flex flex-col flex-1 ">
              <label for="reps" class="mb-1 text-sm text-at-light-green">Reps</label>
              <input v-if="edit" id="reps" type="text" class="w-full text-gray-500 focus:outline-none" v-model="item.reps" />
              <p v-else>{{ item.reps }}</p>
            </div>

            <div class="flex flex-col flex-1 ">
              <label for="weight" class="mb-1 text-sm text-at-light-green">Weight</label>
              <input v-if="edit" id="weight" type="text" class="w-full text-gray-500 focus:outline-none" v-model="item.weight" />
              <p v-else>{{ item.weight }}</p>
            </div>
            <div v-if="edit" class="relative ml-8">
              <svg @click="deleteExercise(item.id)" xmlns="http://www.w3.org/2000/svg" class="absolute right-0 w-auto p-1 text-green-500 bg-white rounded-md cursor-pointer fill-current h-7 top-5" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
              </svg>
            </div>
          </div>
          <button v-if="edit" @click="addExercise" type="submit" class="submit-button">
            Add Exercise
          </button>
        </div>

        <!-- Cardio  -->

        <div v-else class="flex flex-col w-full gap-y-4">
          <div class="relative flex felx-col gap-x-4 gap-y-6 sm:flex-row " v-for="(item, index) in data.exercises" :key="index">
            <div class="flex flex-col flex-2 md:w-1/3">
              <label for="cardioType" class="mb-1 text-sm text-at-light-green">Cardio Type </label>
              <select v-if="edit" id="cardioType" class="w-full text-gray-500 focus:outline-none" v-model="item.cardioType"
                ><option value="#" disabled>Select Type</option>
                <option value="run">Run</option>
                <option value="walk">Walk</option></select
              >
              <p v-else>{{ item.cardioType }}</p>
            </div>

            <div class="flex flex-col flex-1 ">
              <label for="distance" class="mb-1 text-sm text-at-light-green">Distance</label>
              <input v-if="edit" id="distance" type="text" class="w-full text-gray-500 focus:outline-none" v-model="item.distance" />
              <p v-else>{{ item.distance }}</p>
            </div>

            <div class="flex flex-col flex-1 ">
              <label for="duration" class="mb-1 text-sm text-at-light-green">Duration</label>
              <input v-if="edit" id="duration" type="text" class="w-full text-gray-500 focus:outline-none" v-model="item.duration" />
              <p v-else>{{ item.duration }}</p>
            </div>

            <div class="flex flex-col flex-1 ">
              <label for="pace" class="mb-1 text-sm text-at-light-green">Pace</label>
              <input v-if="edit" id="pace" type="text" class="w-full text-gray-500 focus:outline-none" v-model="item.pace" />
              <p v-else>{{ item.pace }}</p>
            </div>
            <div v-if="edit" class="relative ml-7">
              <svg @click="deleteExercise(item.id)" xmlns="http://www.w3.org/2000/svg" class="absolute right-0 w-auto p-1 text-green-500 bg-white rounded-md cursor-pointer fill-current h-7 top-5" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
              </svg>
            </div>
          </div>
          <button v-if="edit" @click="addExercise" type="submit" class="submit-button">
            Add Exercise
          </button>
        </div>
      </div>
      <!-- update workout button-->
      <button
        v-if="edit"
        @click="updateWorkout"
        type="submit"
        class="self-start px-6 py-2 mt-10 text-sm text-white 
  duration-200 border-2 border-transparent border-solid rounded-sm 
  bg-at-light-green hover:border-at-light-green hover:bg-white hover:text-at-light-green hover:border-at-light-green;"
      >
        Update Workout
      </button>
      <ConfirmDialog :isVisible="modalVisible" @cancel="modalVisible = false" @confirm="deleteWorkout" />
    </div>
  </div>
</template>

<script>
import ConfirmDialog from "../components/confirm-dialog.vue"
import { ref, computed } from "vue"

import { supabase } from "../supabase/init"
import { useRoute, useRouter } from "vue-router"
import store from "../store/index"
import { uid } from "uid"
export default {
  components: { ConfirmDialog },
  name: "view-workout",
  setup() {
    // Create data / vars
    const data = ref(null)
    const modalVisible = ref(false)
    const dataLoaded = ref(null)
    const errorMsg = ref(null)
    const statusMsg = ref(null)
    const route = useRoute()
    const router = useRouter()

    // Get current Id of route
    const currentId = route.params.workoutId

    // Get workout data
    const getData = async () => {
      try {
        const { data: workouts, error } = await supabase
          .from("workouts")
          .select("*")
          .eq("id", currentId)
        if (error) throw error
        data.value = workouts[0]
        dataLoaded.value = true
        console.log(data.value)
      } catch (error) {
        errorMsg.value = error.message
        setTimeout(() => {
          errorMsg.value = false
        }, 5000)
      }
    }

    // Get data user from store
    const user = computed(() => store.state.user)

    getData()

    //Delete workout
    const showModal = () => {
      modalVisible.value = true
    }
    const deleteWorkout = async () => {
      try {
        const { error } = await supabase
          .from("workouts")
          .delete()
          .eq("id", currentId)
        if (error) throw error
        router.push({ name: "Home" })
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`
        setTimeout(() => {
          errorMsg.value = false
        }, 5000)
      }
    }
    // Edit mode
    const edit = ref(null)
    const editMode = () => {
      edit.value = !edit.value
    }

    // Add exercise
    const addExercise = () => {
      if (data.value.workoutType.value === "strength") {
        data.value.exercises.push({
          id: uid(),
          exercise: "",
          sets: "",
          reps: "",
          weight: "",
        })
        return
      }
      data.value.exercises.push({
        id: uid(),
        cardioType: "",
        distance: "",
        duration: "",
        pace: "",
      })
    }
    // Delete exercise
    const deleteExercise = (id) => {
      if (data.value.exercises.length > 1) {
        data.value.exercises = data.value.exercises.filter((exercise) => exercise.id !== id)
        return
      }
      errorMsg.value = "Error: can not remove, need at least one exercise. "
      setTimeout(() => {
        errorMsg.value = false
      }, 5000)
    }

    // Update Workout

    const updateWorkout = async () => {
      try {
        const { error } = await supabase
          .from("workouts")
          .update({
            workoutName: data.value.workoutName,
            exercises: data.value.exercises,
          })
          .eq("id", currentId)
        if (error) throw error
        edit.value = false
        statusMsg.value = "Record is updated successfully."
        setTimeout(() => {
          statusMsg.value = false
        }, 5000)
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`
        setTimeout(() => {
          errorMsg.value = false
        })
      }
    }

    return {
      showModal,
      statusMsg,
      data,
      editMode,
      user,
      edit,
      errorMsg,
      dataLoaded,
      addExercise,
      deleteExercise,
      updateWorkout,
      deleteWorkout,
      modalVisible,
    }
  },
}
</script>
<style scoped>
.submit-button {
  @apply self-start px-6 py-2 mt-6 text-sm text-white 
  duration-200 border-2 border-transparent border-solid rounded-sm 
  bg-at-light-green hover:border-white hover:bg-white hover:text-at-light-green hover:border-at-light-green;
}
</style>
