<template>
  <div class="board-list">
    <p>번호: {{ idx }}</p>
    <p>제목: {{ title }}</p>
    <p>작성자: {{ author }}</p>
    <p>내용: {{ contents }}</p>
    <p>작성일: {{ createdAt }}</p>
  </div>

</template>


<script>
export default {

  data() {
    return {
      requestBody: this.$route.query,
      idx: this.$route.query.idx,
      title: '',
      author: '',
      contents: '',
      createdAt: ''
    }
  }, mounted() {
   this.fnGetView();
  }, methods: {
    fnGetView() {
      this.$axios.get(this.$serverUrl + "/board/" + this.idx)
          .then((res) => {
            console.log(res.data)
            this.title = res.data.title
            this.author = res.data.author
            this.contents = res.data.contents
            this.createdAt = res.data.createdAt
          })
          .then((err) => {
            console.log(err)
          })
    },
  }
}
</script>

<style scoped>
.board-list {
  width: 400px;
  margin: auto;
}
</style>