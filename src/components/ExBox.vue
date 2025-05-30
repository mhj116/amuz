<template>
  <div>
    <h1>안녕하세요 사용자 {{ name }} !</h1>
    <h1>{{ nm }}</h1>
    <button @click="BtNum">youtbe 컴포넌트</button>
  </div>
</template>

<script>
export default {
  name: "ExBox",
  props: {
    name: String,
    nm: Number,
    BtNum: Function,
  },
};
</script>

<style>
</style>