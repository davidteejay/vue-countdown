<template>
  <div class="countdown">
    <h1 v-if="!timeUp">{{ minute }}:{{ second < 10 ? '0' + second : second }}</h1>
    <h1 v-else>Time's up</h1>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data(){
    return {
      minute: 20,
      second: 0,
      timeUp: false
    }
  },
  mounted(){
    let timer = setInterval(() => {
      let { minute, second } = this

      if (second === 0 && minute === 0){
        clearInterval(timer)
        this.timeUp = true
      } else {
        if (second === 0){
          second = 59
          minute--
        } else second--

        this.second = second
        this.minute = minute
      }
    }, 1000)
  }
}
</script>

<style scoped>
.countdown {
  display: flex;
  align-items: center;
  justify-content: center
}

h1 {
  width: 100%
}
</style>
