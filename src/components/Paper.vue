<template>
  <div class="paper">
    <div class="title">我要吐个槽：</div>
    <textarea 
      class="words-textarea"
      :class="{error: error}"
      placeholder="说点儿什么吧~"
      name="words"
      autofocus
      v-model="words"
    ></textarea>
    <div class="action">
      <span class="btn" @click="doAction">收起来吧~</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'paper',
  data() {
    return {
      words: '',
      error: false
    }
  },
  methods: {
    doAction(){
      if (this.words === '') {
        this.error = true
        setTimeout(() => {
          this.error = false
        }, 2000)
        return
      }
      this.$emit('action')
      // 清空吐槽内容
      setTimeout(() => {
        this.words = ''
      }, 1000)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .paper{
    background: url('../assets/paper.png') no-repeat;
    background-size: cover;
    overflow: hidden;
    font-style: italic;
    padding: 16% 0;
    height: 100%;
    box-sizing: border-box;
  }
  .title{
    margin: 5px 0 5px 15%;
    font-size: 18px;
    color: #fff;
    text-align: left;
  }
  .words-textarea{
    width: 70%;
    height: 70%;
    padding: 5px;
    border-radius: 5px;
    font-size: 14px;
    color: #946534;
    text-indent: 2em;
    resize: none;
  }
  .words-textarea, .words-textarea:focus{
    border: none;
    outline: none;
  }
  .words-textarea.error{
    box-shadow: 0px 0px 5px 1px #c81315;
  }
  .action{
    margin-top: 5px;
  }
  .btn{
    font-size: 12px;
    color: #9c7140;
    padding: 5px 20px;
    border: 1px solid #dcbb95;
    border-radius: 6px;
    background: #faf0e2;
    cursor: pointer;
  }
  .btn:hover{
    box-shadow: 0px 0px 5px 1px #9c7140;
  }
</style>
