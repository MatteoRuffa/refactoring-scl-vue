<template>
    <textarea
      class="text-area"
      :placeholder="placeholder"
      :maxlength="maxLength"
       @input="$emit('update:modelValue', $event.target.value)"
    ></textarea>    <!-- v-model="store.feedbackData.choices.additionalComment" -->
  </template>
  
  <script>
  
  export default {
    name: "TextArea",
    props: {
      placeholder: {
        type: String,
      },
      maxLength: {
        type: Number,
      },
      modelValue: {
        type: String, 
        default: "",
        required: true,
      },
    },
    data() {
      return {
      };
    },
    mounted() {
    //   console.log("modelValue:", this.modelValue);
    },
    watch: {
    //   "store.feedbackData.additionalComments": {
    //     immediate: true,
    //     handler(newValue) {
    //       console.log("Aggiornato additionalComments:", newValue);
    //     },
    //   },
    },
  };
  </script>
  
  <style lang="scss" scoped>
  .text-area {
    width: 80%;
    min-height: 120px;
    border: 1px solid #ccc;
    border-radius: 5px;
    padding: 0.8rem;
    font-size: 1rem;
    resize: none;
    box-shadow: 0 0 10px rgba(0.1, 0.1, 0.1, 0.1);
  }
  </style>
  