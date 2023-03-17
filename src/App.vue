<script setup lang="ts">
import { reactive } from 'vue';
import Card from './components/Card.vue';
import CardForm from './components/CardForm.vue';

const state = reactive({
  cardNumber: "",
  cardName: "",
  cardMonth: "",
  cardYear: 0,
  cardCvv: "",
  cardFlip: false
});

type InputChangeArgs = {
  name: string;
  value: string
};

function handleInputChange({name, value}: InputChangeArgs) {
  console.log(name, value);
  state[name] = value;
}

function handleFlipCard(toFlip: boolean) {
  state.cardFlip = toFlip;
}

</script>

<template>
  <div class="wrapper" id="app">
    <div class="card-form">
      <div class="card-list">
        <Card :cardNumber="state.cardNumber" :cardName="state.cardName" :cardMonth="state.cardMonth" :cardYear="state.cardYear" :cardCvv="state.cardCvv" :cardFlip="state.cardFlip"/>
      </div>
      <CardForm @inputChanged="handleInputChange" @cvvFocus="handleFlipCard(true)" @cvvBlur="handleFlipCard(false)"/>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  min-height: 100vh;
  display: flex;
  padding: 50px 15px;
}

.card-form {
  max-width: 570px;
  margin: auto;
  width: 100%;
}

.card-list {
  margin-bottom: -130px;

  @media screen and (max-width: 480px) {
    margin-bottom: -120px;
  }
}
</style>
