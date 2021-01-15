<template>
  <div class="container column">
    <AppResumeForm @formSubmit="addBlock" />
    <AppResumeBlocks :blocks="blocks"/>
  </div>
  <div class="container">
    <p>
      <AppButton btnColor="primary" @action="loadComments">Загрузить комментарии</AppButton>
    </p>
    <AppLoader v-if="loading"></AppLoader>
    <AppComments v-else @action="loadComments" :loading="loading" :comments="comments" />
  </div>
</template>

<script>
import AppResumeForm from './AppResumeForm'
import AppResumeBlocks from './AppResumeBlocks'
import AppComments from './AppComments'
import AppButton from './AppButton'
import AppLoader from './AppLoader'
export default {
  data() {
    return {
      blocks: [],
      comments: null,
      loading: false
    }
  },
  methods: {
    addBlock(e) {
      this.blocks.push({type: e.type, value: e.value})
    },
    async loadComments() {
      this.loading = true
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=42')
        const data = await response.json()
        this.comments = Object.keys(data).map(key => {
          return { ...data[key], id: key }
        })
      } catch (e) {
        console.log(e)
      }
      this.loading = false
    }
  },
  components: {AppResumeForm, AppResumeBlocks, AppComments, AppButton, AppLoader}
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
