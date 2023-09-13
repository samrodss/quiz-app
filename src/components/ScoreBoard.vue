<template>
  <section class="score">
    <h2>
      PLAYER <span>{{ this.winnerCount }}</span>
    </h2>
    <p>X</p>
    <h2>
      <span>{{ this.loserCount }}</span
      >Computer
    </h2>
  </section>
</template>

<script>
export default {
  name: 'ScoreBoard',

  props: ['winnerCount', 'loserCount']
}
</script>

<style lang="scss">
section.score {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  border-bottom: 1px solid black;
  padding: 24px;
  font-size: 18px;

  span {
    padding: 8px;
    font-size: 2rem;
    font-weight: bold;
    border-radius: 1rem;
  }

  h2 {
    display: flex;
    align-items: center;
    text-align: center;

    @media (max-width: 600px) {
      margin: auto;
      font-size: 1.2rem;
    }
  }
}
</style>
