<template>
   <section class="container">
      <div class="questionBox" id="app">
         <quizBody></quizBody>
      </div>
   </section>
</template>

<script>
   import quizBody from './quizBody.vue';
   export default {
      components: {
         quizBody
      },
   };
</script>
