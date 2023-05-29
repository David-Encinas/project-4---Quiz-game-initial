<template>
  <div>
    <section class="score">
      Player <span>{{ this.winCount }}</span> x <span>{{ this.loseCount }}</span> Computer
    </section>
  </div>
</template>

<script>
export default {
    props: ['winCount', 'loseCount']
}
</script>

<style>
</style>