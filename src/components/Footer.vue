<template>
  <footer>
    <p>&copy; {{ new Date().getFullYear() }}</p>
  </footer>
</template>

<script>
export default {}
</script>

<style scope>
footer {
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  background-color: rgb(86, 84, 212);
  color: white;
  font-size: medium;
  text-align: center;
  padding: 10px;
}
</style>
