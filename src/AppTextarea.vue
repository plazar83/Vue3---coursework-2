<template>
    <div class="form-control">
        <label :for="id">{{label}}</label>
        <textarea :id="id" :value="modelValue" :placeholder="placeholder" @input="change" rows="3"></textarea>
    </div>
</template>

<script>
  export default {
    emits: ['input'],
    props: {
      label: String,
      placeholder: String,
      error: String,
      modelValue: String
    },
    data() {
      return {
        id: Math.random()
      }
    },
    methods: {
      change(event) {
        this.$emit('input', event.target.value)
      }
    }
  }
</script>