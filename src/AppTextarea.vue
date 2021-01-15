<template>
    <div class="form-control">
        <label :for="ID">{{label}}</label>
        <textarea :id="ID" :value="modelValue" :placeholder="placeholder" @input="change" rows="3"></textarea>
    </div>
</template>

<script>
  export default {
    emits: ['input'],
    props: {
      label: String,
      placeholder: String,
      modelValue: String
    },
    methods: {
      change(event) {
        this.$emit('input', event.target.value)
      }
    },
    computed: {
      ID() {
        return '_' + Math.random().toString(36).substr(2, 9)
      }
    }
  }
</script>