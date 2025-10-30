<template>
  <div id="app">
    <table>
      <tbody>
        <tr>
          <td>
            <label for="sideLength">
              sideLength:
              <input v-model="sideLength" id="sideLength" type="number" min="0" />
            </label>
          </td>
          <td>
            <label for="strokeWidth">
              strokeWidth:
              <input v-model="strokeWidth" id="strokeWidth" type="number" min="0" />
            </label>
          </td>
        </tr>
        <tr>
          <td>
            <label for="circleCount">circleCount:</label>
          </td>
          <td>
            <input v-model="circleCount" id="circleCount" type="number" min="0" />
          </td>
        </tr>
      </tbody>
    </table>

    <ul>
      <li v-for="(segment, index) of segments" :key="index">
        <label>
          count:
          <input v-model="segment.count" type="number" min="0" />
        </label>
        <label>
          color:
          <input v-model="segment.color" type="color" />
        </label>
        <button title="Удалить сегмент" @click="segments.splice(index, 1)">+</button>
      </li>
    </ul>
    <button
      title="Добавить сегмент"
      @click="segments.push({ count: 10, color: segments.length % 2 ? '#000000' : '#808080' })"
    >
      +
    </button>

    <CharMulti :array="segments" :circleCount="+circleCount" :sideLength="sideLength" :strokeWidth="strokeWidth" />
  </div>
</template>

<script>
import CharMulti from './components/CharMulti.vue'

export default {
  id: 'App',
  components: { CharMulti },

  data: () => ({
    circleCount: 100,
    segments: [
      { count: 10, color: '#013A76' },
      { count: 20, color: '#FFCC00' },
      { count: 30, color: '#2BC57B' },
      { count: 40, color: '#DB3B2F' }
    ],

    sideLength: 200,
    strokeWidth: 40
  })
}
</script>

<style lang="scss">
html,
body {
  min-height: 100%;
  display: flex;
  flex-direction: column;
}
body {
  flex-grow: 1;
  margin: 0;
  display: flex;
  flex-direction: column;
}
#app {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 20px;
}

table {
  font-size: 20px;
}
td {
  width: 50%;
}
td:first-child {
  text-align: right;
  padding: 8px 8px 8px 0;
}
input[type='number'],
input[type='color'] {
  font-size: 16px;
  width: 60px;
  box-sizing: border-box;
}

button {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  font-size: 28px;
  cursor: pointer;
  transition-duration: 150ms;

  &:hover {
    background-color: #bababa;
  }
}

ul {
  margin: 16px 0 8px;
  list-style: none;
  padding: 0;
}
li {
  display: flex;
  align-items: center;
  gap: 12px;

  & + & {
    margin-top: 8px;
  }

  button {
    rotate: 45deg;
  }
}

button:not(:last-child) {
  margin-bottom: 20px;
}
</style>
