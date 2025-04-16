<template>
  <div id="app">
    <p>{{ message }}</p>
    <section>
      <button @click="handleClick">버튼 1</button>
      <br>
      <button @click="openGoogle">google</button>
      <br>
      <button @click="openNaver">naver</button>
      <br>
      <button @click="openYoutube">youtube</button>
      
    </section>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      message: 'Hello, Vue!!'
    };
  },
  methods: {
    handleClick() {
      this.message = '이 버튼은 클릭됐어!';
    },
    openGoogle() {
      window.open("https://www.google.com", "_blank");
    },
    openNaver() {
      window.open("https://www.naver.com", "_blank");
    },
    openYoutube() {
      window.open("https://www.youtube.com", "_blank");
    }
  }
}
</script>
<style scoped>
button {
  margin: 5px;
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
}
</style>