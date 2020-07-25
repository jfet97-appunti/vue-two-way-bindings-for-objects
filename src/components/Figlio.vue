<template>
  <div>
    <h1>Figlio</h1>
    <pre>
      <code>
        {{ formattedObj }}
      </code>
    </pre>
    <button @click="randomUpdateObj">Aggiorna k2 dal figlio</button>
  </div>
</template>

<script>
export default {
  name: "Figlio",
  props: {
    obj: {
      type: Object,
      required: true
    }
  },
  computed: {
    formattedObj() {
      return JSON.stringify(this.obj, null, 4);
    }
  },
  methods: {
    randomUpdateObj() {
      const newK2 = Math.floor(Math.random() * (100 - 1) + 1);
      this.$emit("update:obj", { ...this.obj, k2: newK2 });
    }
  }
};
</script>

