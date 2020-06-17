<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>Demo Quiz app</b-nav-item>
      <b-nav-item disabled>Counter: {{ numCorrectAnswers}}/{{numTotal}} </b-nav-item>
    </b-nav>
  </div>
</template>

<script>
    export default {
        name: "Header",
        props: [
            'numCorrectAnswers',
            'numTotal'
        ]
    }
</script>

<style scoped>

</style>
