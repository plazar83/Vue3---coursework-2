<template>
    <button class="btn" :class="btnColor" @click="$emit('action')"><slot /></button>
</template>
<script>
  export default {
    name: 'AppButton',
    emits: ['action'],
    props: {
      btnColor: {
        type: String,
        default: '',
        validator: function(value) {
          return ['', 'primary', 'danger'].includes(value)
        }
      }
    }
  }
</script>