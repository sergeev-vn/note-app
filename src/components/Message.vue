<template>
  <div class="message">
    <p>{{mess}}</p>
  </div>
</template>

<script>
export default {
  props: {
    mess: {
      type: String,
      required: true,
    }
  }
}
</script>

<style lang="postcss" scoped>
  p {
    color: red;
    text-align: center;
    padding: 20px;
  }
</style>