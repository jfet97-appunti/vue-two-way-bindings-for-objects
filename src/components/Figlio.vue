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
    localObj: {
      get() {
        return JSON.parse(JSON.stringify(this.obj));
      },
      set(newObj) {
        this.$emit("update:obj", newObj);
      }
    },
    formattedObj() {
      return JSON.stringify(this.localObj, null, 4);
    }
  },
  methods: {
    randomUpdateObj() {
      const newK2 = Math.floor(Math.random() * (100 - 1) + 1);
      this.localObj = { ...this.localObj, k2: newK2 };
    }
  }
};
</script>

