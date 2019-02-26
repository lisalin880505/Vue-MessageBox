<template>
  <div class="hello">
    <button @click="toast">Toast</button>
    <button @click="alert">Alert</button>
    <button @click="confirm">Confirm</button>
    <button @click="prompt">Prompt</button>
    <button @click="prompt2">使用$MessageBox</button>
  </div>
</template>
<script>
import MessageBox from '../components/MessageBox/index'
export default {
  name: 'Index',
  data () {
    return {
    }
  },
  methods: {
    toast () {
      MessageBox.toast('这个是Toast')
    },
    alert () {
      MessageBox.alert('这个是alert')
    },
    confirm () {
      MessageBox.confirm('确定提交吗').then(() => {
        MessageBox.alert('用户点击确定')
      }).catch(() => {
        MessageBox.alert('用户点击取消')
      })
    },
    prompt () {
      MessageBox.prompt('这个是Prompt').then((obj) => {
        MessageBox.alert('用户输入的是' + obj.value)
      }).catch(() => {
        MessageBox.alert('用户点击取消')
      })
    },
    prompt2 () {
      this.$MessageBox.prompt('这个是Prompt').then((obj) => {
        this.$MessageBox.alert('用户输入的是' + obj.value)
      }).catch(() => {
        this.$MessageBox.alert('用户点击取消')
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
button {
  display: block;
  width: 80%;
  line-height: 35px;
  margin: auto auto 20px auto;
  border: 1px solid #42b983;
  color: #42b983;
  background: transparent;
  border-radius: 5px;
  outline: none;
}
</style>
