<template>
  <div class="board-list">
    <p>제목: <input v-model="title" id="title"></p>
    <p>작성자: <input v-model="author" id="author"></p>
    <p>내용: <input v-model="contents" id="contents"></p>
    <p>작성일: <input v-model="createdAt" id="createdAt"></p>
  </div>
  <div><button v-on:click="fnSave">저장</button></div>

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

    if(this.idx != null) {
        this.fnGetView();
    }
   
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
    fnSave() {
      let apiUrl = this.$serverUrl + '/board'
        this.form = {
          "idx": this.idx,
          "title": this.title,
          "contents": this.contents,
          "author": this.author
        }
        if(this.idx != null) {
                //UPDATE
                this.$axios.patch(apiUrl, this.form)
                    .then((res) => {
                      alert('글이 저장되었습니다.')
                      this.fnView(res.data.idx)
                    }).catch((err) => {
                  if (err.message.indexOf('Network Error') > -1) {
                    alert('네트워크가 원활하지 않습니다.\n잠시 후 다시 시도해주세요.')
                  }
                })
            } else {
              this.$axios.post(apiUrl, this.form)
                    .then((res) => {
                      alert('글이 저장되었습니다.')
                      this.fnView(res.data.idx)
                    }).catch((err) => {
                  if (err.message.indexOf('Network Error') > -1) {
                    alert('네트워크가 원활하지 않습니다.\n잠시 후 다시 시도해주세요.')
                  }
                })
            }

         
    }
  }
}
</script>

<style scoped>
.board-list {
  width: 700px;
  margin: auto;
}
</style>