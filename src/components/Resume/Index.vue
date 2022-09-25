<template>
  <main>
    <p>{{ labelVisual }}</p>
    <h1>{{ amountVisual }}</h1>
  </main>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      default: null,
    },
    totalLabel:{
      type: String, 
      
    },
    totalAmount: {
      type: Number,
    },
    amount: {
      type: Number,
      default: null,
    },
  },
  computed: {
    amountVisual(){
        //si el amount no es (!==) null entonces(?) muestra el monto, y si no (:) muestra el monto total
        return this.amount !== null ? this.amount : this.totalAmount;
    },
    labelVisual(){
        return this.label !== null ? this.label : this.totalLabel;
    }
  }
};
</script>

<style scope>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}
h1,
p {
  margin: 0;
  text-align: center;
}
h1 {
  margin-top: 14px;
  color: var(--brand-green);
}

.graphic {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 48px 24px;
  box-sizing: border-box;
}
</style>
