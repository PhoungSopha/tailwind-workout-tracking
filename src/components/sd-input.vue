<template>
  <div>
    <label v-if="label" :for="id" class="mb-2 font-semibold text-gray-700 form-label">{{ label }}</label>

    <div class="relative">
      <div
        :id="id"
        ref="input"
        v-bind="$attrs"
        :type="type"
        :value="value"
        @input="$emit('input', $event.target.value)"
        @keyup="$emit('keyup', $event)"
        @blur="$emit('blur', $event)"
        @keydown="$emit('keydown', $event)"
        :class="[
          {
            'border-red-400': errors.length,
            'pl-12': withIcon === true,
          },
        ]"
        class="block w-full h-2 px-2 py-2 font-sans leading-normal text-left text-gray-800 bg-white rounded-lg outline-none appearance-none"
      >
        <div v-if="errors.length" class="mt-1 text-sm text-red-500">
          {{ errors[0] }}
        </div>

        <svg class="absolute text-red-600 fill-current" style="top: 12px; right: 12px" v-if="errors.length" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
          <path d="M11.953,2C6.465,2,2,6.486,2,12s4.486,10,10,10s10-4.486,10-10S17.493,2,11.953,2z M13,17h-2v-2h2V17z M13,13h-2V7h2V13z" />
        </svg>

        <div class="absolute "></div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    label: { type: String, default: "" },
    value: { type: String, default: "" },
  },
}
</script>

<style></style>
