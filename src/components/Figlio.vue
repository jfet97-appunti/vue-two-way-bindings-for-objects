<template>
  <div>
    <h1>Figlio</h1>
    <pre>
      <code>
        {{ formattedObj }}
      </code>
    </pre>
    <input type="text" v-model="localObj.k1">
    <button @click="randomUpdateObj">Aggiorna k2 dal figlio</button>
  </div>
</template>

<script>
// ATTENZIONE
// le reference this.localObj e this.oldLocalObj
// non vanno mai modificate
// solo i watch che ho scritto hanno l'autorizzazione
// per farlo

// this.oldLocalObj va considerato readonly, in quanto oggetto
// this.localObj è mutabile e scatena l'aggiornamento di this.obj
export default {
  name: "Figlio",
  props: {
    obj: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
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
        // Qnd la prop this.obj muta, in quanto reference o in quanto oggetto con
        // proprietà (ecco il perché del deep:true), creiamo un clone mutabile di
        // obj modificabile localmente, oltre che tenere in memoria la precedente
        // versione del clone locale mutabile
        // La precedente versione ci serve perché this.obj potrebbe mutare proprio
        // a causa di una mutazione eseguita sul clone locale:
        // this.localObj viene modificato -> scatta il watch su localObj ->
        // ipotesi: localObj differisce rispetto alla versione prec -> evento ->
        // this.obj viene aggiornato, diventa una copia separata di il this.localObj
        // -> questo watch scatta -> this.oldLocalObj diventa il this.localObj,
        // proprio la nuova versione che era stata clonata per aggiornare this.obj
        // -> this.localObj viene clonato a partire da newObj aka this.obj,
        // che era una copia proprio di this.localObj, ergo this.localObj è in sostanza
        // identico a prima -> scatta il watch su localObj -> localObj non differisce più,
        // in quanto a sostanza, rispetto alla versione precedente -> nessun evento emesso
        // il ciclo si interrompe

        this.oldLocalObj = this.localObj;
        this.localObj = JSON.parse(JSON.stringify(newObj));
      }
    },
    localObj: {
      deep: true,
      handler(newLocalObj) {
        // if(
        // logica custom che determina se newLocalObj è diverso da this.oldLocalObj
        // )
        if (
          this.oldLocalObj === null ||
          newLocalObj.k1 !== this.oldLocalObj.k1 ||
          newLocalObj.k2 !== this.oldLocalObj.k2
        ) {
          // in tal caso, aggiorniamo this.obj
          this.$emit("update:obj", JSON.parse(JSON.stringify(newLocalObj)));
        }
      }
    }
  }
};
</script>

