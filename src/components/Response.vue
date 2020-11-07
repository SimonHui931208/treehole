<template>
  <div class="response">
    <img src="../assets/cloud.png" alt="白云" class="cloud">
    <div class="content">
      懂你，加油
    </div>
    <div class="action-btn" @click="doAction">我还要吐槽</div>
  </div>
</template>

<script>
export default {
  name: 'response',
  methods: {
    doAction(){
      this.$emit('action')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .response{
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 30%;
    height: 30%;
    z-index: 1011;
    background-color: #f7f7f7;
    border-radius: 20px;
    box-shadow: 0px 0px 5px 1px #cdcfe6;
  }
  .cloud{
    position: absolute;
    top: 0;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 50%;
  }
  .content{
    margin-top: 20%;
    font-size: 30px;
    font-style: italic;
    color: #f37e41;
  }
  .action-btn{
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    bottom: 30px;
    padding: 10px;
    border-radius: 4px;
    background-color: #cc4547;
    color: #fff;
    cursor: pointer;
    font-size: 14px;
  }
  .action-btn:hover{
    box-shadow: 0px 0px 5px 1px #ce8182;
  }
</style>
