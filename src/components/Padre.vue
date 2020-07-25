<template>
  <div>
    <h1>Padre</h1>
    <pre>
      <code>
        {{ formattedObj }}
      </code>
    </pre>
    <input type="text" v-model="obj.k1">
    <button @click="randomUpdateObj">Aggiorna k2 dal padre</button>
    <Figlio v-bind.sync="obj"/>
  </div>
</template>

<script>
import Figlio from "./Figlio";

export default {
  name: "Padre",
  components: {
    Figlio
  },
  data() {
    return {
      obj: {
        k1: "val1",
        k2: 2
      }
    };
  },
  computed: {
    formattedObj() {
      return JSON.stringify(this.obj, null, 4);
    }
  },
  methods: {
    randomUpdateObj() {
      this.obj.k2 = Math.floor(Math.random() * (100 - 1) + 1);
    }
  }
};
</script>

