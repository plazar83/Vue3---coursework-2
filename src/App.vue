<template>
  <div class="container column">
    <AppResumeForm @formSubmit="addBlock" />
    <AppResumeBlocks :blocks="blocks"/>
  </div>
  <div class="container">
    <AppComments />
  </div>
</template>

<script>
import AppResumeForm from './AppResumeForm'
import AppResumeBlocks from './AppResumeBlocks'
import AppComments from './AppComments'

export default {
  data() {
    return {
      blocks: []
    }
  },
  methods: {
    addBlock(e) {
      this.blocks.push({type: e.type, value: e.value})
    },
  },
  components: {AppResumeForm, AppResumeBlocks, AppComments}
}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>
