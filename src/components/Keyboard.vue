
<script>
export default {
  name: 'TheKeyboard',

  data () {
    return {
      buttonList: ['AC', '+/-', '%', '/', '7', '8', '9', '*', '4', '5', '6', '-', '1', '2', '3', '+', '0', ',', '=']
    }
  }
}
</script>

<template>
  <div class="button">
    <button
      v-for="button in buttonList"
      :key="button"
      @click="$emit('tap', button)"
    >{{ button === '*' ? 'x' : button }}
    </button>
  </div>
</template>

<style scoped>
  .button {
    width: 100%;
    height: 60%;
    background: #000000;
    display: grid;
    grid-template-columns: 25% 25% 25% 25%;
  }
  button {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 90px;
    height: 90px;
    padding: 10px;
    font-size: 36px;
    background: #343434;
    color: white;
    border-radius: 50%;
    border: none;
    margin-left: 4px;
    margin-bottom: 8px;
    cursor: pointer;
  }
  button:nth-child(1), button:nth-child(2), button:nth-child(3) {
    background: #a5a5a5;
  }
  button:nth-child(4), button:nth-child(8), button:nth-child(12), button:nth-child(16), button:nth-child(19) {
    background: #ff9f0a;
  }
  button:nth-child(17) {
    padding-inline: 95px;
    border-radius: 33%;
    grid-column: span 2;
  }
  button:nth-child(17) {
    margin-left: 8px;
  }

</style>
