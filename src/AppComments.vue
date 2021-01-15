<template>
    <p>
        <AppButton btnColor="primary" @action="loadComments">Загрузить комментарии</AppButton>
    </p>
    <AppLoader v-if="loading"></AppLoader>
    <div v-else-if="comments" class="card">
        <h2>{{title}}</h2>
        <ul class="list">
            <li class="list-item" v-for="comment in comments" :key="comment.id">
                <div>
                    <p><strong>{{comment.email}}</strong></p>
                    <small>{{comment.body}}</small>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
  import AppButton from './AppButton'
  import AppLoader from './AppLoader'
  export default {
    data() {
      return {
        title: 'Комментарии',
        comments: null,
        loading: false
      }
    },
    methods: {
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
    components: {AppLoader, AppButton}
  }
</script>

<style scoped>
   .list-item + li {
       border-top: 1px solid #ccc;
   }
</style>