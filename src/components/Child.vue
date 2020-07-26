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
      // the reference needs to be changed to trigger the reference
      // changing a prop is not enough
      this.localObj = { ...this.localObj, k2: newK2 };
    }
  }
};
</script>

