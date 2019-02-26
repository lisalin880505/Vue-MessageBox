<template>
  <div class="hello">
    <button @click="toast">Toast</button>
    <button @click="alert">Alert</button>
    <button @click="confirm">Confirm</button>
    <button @click="prompt">Prompt</button>
  </div>
</template>
<script>
export default {
  name: 'Index',
  data () {
    return {
    }
  },
  methods: {
    toast () {
      this.$MessageBox.toast('这个是Toast')
    },
    alert () {
      this.$MessageBox.alert('这个是alert')
    },
    confirm () {
      this.$MessageBox.confirm('确定提交吗').then(() => {
        this.$MessageBox.alert('用户点击确定')
      }).catch(() => {
        this.$MessageBox.alert('用户点击取消')
      })
    },
    prompt () {
      this.$MessageBox.prompt('这个是$MessageBox.prompt').then((obj) => {
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
