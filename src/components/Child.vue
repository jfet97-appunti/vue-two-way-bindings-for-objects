<template>
  <div>
    <h1>Child</h1>
    <pre>
      <code>
        {{ formattedObj }}
      </code>
    </pre>
    <button @click="randomUpdateObj">Update k2 from child</button>
  </div>
</template>

<script>
export default {
  name: "Child",
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

