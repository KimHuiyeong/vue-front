<template>
  <div class="board-list">
    <p>번호: {{ idx }}</p>
    <p>제목: {{ title }}</p>
    <p>작성자: {{ author }}</p>
    <p>내용: {{ contents }}</p>
    <p>작성일: {{ createdAt }}</p>
  </div>
  <div>
    <button v-on:click="fnUpdate">수정</button>
    <button v-on:click="fnDelete">삭제</button>
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
    },fnUpdate() {
      this.$router.push({
        path: './write',
        query: this.requestBody
      })
    }, fnDelete (){
      if (!confirm("삭제하시겠습니까?")) return

      this.$axios.delete(this.$serverUrl + '/board/' + this.idx, {})
          .then(() => {
            alert('삭제되었습니다.');
            this.fnList();
          }).catch((err) => {
        console.log(err);
      })
    }
  }
}
</script>

<style scoped>
.board-list {
  width: 400px;
  margin: auto;
}
</style>