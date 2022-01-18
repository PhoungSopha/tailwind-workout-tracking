<template>
  <div v-if="isVisible">
    <div class="fixed inset-0 z-50 flex items-center justify-center">
      <div
        class="flex flex-col max-w-5xl bg-white rounded-lg shadow-lg"
      >
        <!-- Header -->
        <div class="p-5 ">
          <div class="flex items-start justify-between">
            <h3 class="text-2xl font-semibold text-at-light-green">
              Delete confirm
            </h3>
            <button class="p-1 leading-none" @click="emitCancel">
              <div class="w-6 h-6 text-xl font-semibold">
                <span class="px-1.5 rounded-full text-at-light-green"
                  >X</span
                >
              </div>
            </button>
          </div>
        </div>
        <hr />
        <!-- Body -->
        <div class="p-6">
          <p>Are you sure you want to delete this record?</p>
        </div>
        <!-- footer  -->
        <div class="flex items-center justify-end p-6">
          <button
            @click="emitCancel"
            class="px-4 py-2 text-black border border-gray-500 rounded-md shadow-md outline-none font-sm hover:text-white hover:bg-gray-800"
          >
            Cancel
          </button>
          <button
            @click="emitConfirm"
            class="px-4 py-2 ml-3 text-white bg-red-500 rounded-md shadow-md outline-none font-sm hover:bg-red-700"
          >
            Delete
          </button>
        </div>
      </div>
    </div>

    <div class="fixed inset-0 z-40 bg-black opacity-25 insect"></div>
  </div>
</template>
<script>
export default {
  props: { isVisible: { type: Boolean } },

  setup(props, { emit }) {
    const emitCancel = () => {
      emit("cancel");
    };
    const emitConfirm = () => {
      emit("confirm");
    };
    return { emitCancel, emitConfirm };
  },
};

//   methods: {
//     emitCancel() {
//       this.$emit("cancel");
//     },

//     emitConfirm() {
//       this.$emit("confirm");
//     },
//   },
// };
</script>
