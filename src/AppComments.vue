<template>
    <div v-if="comments" class="card">
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
  export default {
    props: ['comments', 'loading'],
    data() {
      return {
        title: 'Комментарии'
      }
    }
  }
</script>

<style scoped>
   .list-item + li {
       border-top: 1px solid #ccc;
   }
</style>