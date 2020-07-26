<template>
  <div>
    <h1>Parent</h1>
    <pre>
      <code>
        {{ formattedObj }}
      </code>
    </pre>
    <button @click="randomUpdateObj">Update k2 from parent</button>
    <Child :obj.sync="obj"/>
  </div>
</template>

<script>
import Child from "./Child";

export default {
  name: "Parent",
  components: {
    Child
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

