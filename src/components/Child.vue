<template>
  <div>
    <h1>Child</h1>
    <pre>
      <code>
        {{ formattedObj }}
      </code>
    </pre>
    <input type="text" v-model="localK1">
    <button @click="randomUpdateK2">Update k2 from child</button>
  </div>
</template>

<script>
export default {
  name: "Figlio",
  props: {
    k1: {
      type: String,
      required: true
    },
    k2: {
      type: Number,
      required: true
    }
  },
  computed: {
    formattedObj() {
      return JSON.stringify({ k1: this.localK1, k2: this.k2 }, null, 4);
    },
    localK1: {
      get() {
        return this.k1;
      },
      set(newK1) {
        this.$emit("update:k1", newK1);
      }
    }
  },
  methods: {
    randomUpdateK2() {
      const newK2 = Math.floor(Math.random() * (100 - 1) + 1);
      this.$emit("update:k2", newK2);
    }
  }
};
</script>

