<template lang="pug">
.wrapper
  wysiwyg(v-model="msg")

  pre {{msg}}
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      msg: `<p>Welcome to <b>vue-wysiwyg</b>!</p>
      <p>
        <br>
      </p>
      <br>
      <p>
        The editor is quite fast and lightweight. Elements are minimally styled.
      </p>
      `
    }
  }
}

</script>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.wrapper {
  max-width: 800px;
  margin: 10em auto;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  font-family: -apple-system,
    BlinkMacSystemFont,
    "Segoe UI",
    Roboto,
    Oxygen-Sans,
    Ubuntu,
    Cantarell,
    "Helvetica Neue",
    sans-serif;
}

</style>

