<template>
  <div>
    <h1>Child</h1>
    <pre>
      <code>
        {{ formattedObj }}
      </code>
    </pre>
    <input type="text" v-model="localObj.k1">
    <button @click="randomUpdateObj">Update k2 from child</button>
  </div>
</template>

<script>
// WARNING
// this.localObj and this.oldLocalObj must be trated as const references outside
// the watchers that handle their updates

// this.oldLocalObj must be treated as a readonly object
// this.localObj is mutable; when it's updated by this component the parent component will properly update the original object
export default {
  name: "Child",
  // model is customized
  model: {
    prop: "obj",
    event: "update:obj"
  },
  props: {
    obj: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      // we store a local, mutable clone of the original object and we
      // store the last previous version of the local mutable clone to check
      // if there is a difference between that and the current clone

      // that will prevent the infinite loop: clone mutation -> event -> prop update -> clone mutation -> event ...
      // where the initial clone mutation will be an inner mutation (a property that is changed),
      // while the others are reference mutation caused by the obj's watcher

      // deep clone because this.obj is a reference to the original object
      // and we must not mutate it
      localObj: JSON.parse(JSON.stringify(this.obj)),
      oldLocalObj: null
    };
  },
  computed: {
    formattedObj() {
      return JSON.stringify(this.localObj, null, 4);
    }
  },
  methods: {
    randomUpdateObj() {
      const newK2 = Math.floor(Math.random() * (100 - 1) + 1);
      this.localObj.k2 = newK2;
    }
  },
  watch: {
    obj: {
      deep: true,
      handler(newObj) {
        this.oldLocalObj = this.localObj;
        this.localObj = JSON.parse(JSON.stringify(newObj));
      }
    },
    localObj: {
      deep: true,
      handler(newLocalObj) {
        // if(
        //     custom logic that determines if newLocalObj, aka this.localObj,
        //     is different than this.oldLocalObj
        // )
        if (
          this.oldLocalObj === null ||
          newLocalObj.k1 !== this.oldLocalObj.k1 ||
          newLocalObj.k2 !== this.oldLocalObj.k2
        ) {
          // in that case, we emit the event that is going to update the original object
          // this.localObj is cloned because the emitted object will be directly assigned
          // in place of the older original object
          this.$emit("update:obj", JSON.parse(JSON.stringify(newLocalObj)));
        }
      }
    }
  }
};
</script>

