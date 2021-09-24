<template>
  <div>
    <label
      class="answer"
      :for="index"
      v-for="(risposta, index) in domande[domandaCorrente].risposte"
      :key="index"
      :class="{
        'hover border-grey': event == '',
        'bg-red': event == index,
        'bg-green':
          index == domande[domandaCorrente].rispostaCorretta &&
          event != '',
      }"
    >
      <input
      v-model="event"
        type="radio"
        :checked="index == selected"
        :name="index"
        :id="index"
        class="hidden"
        :value="index"
        @change="$emit('selectAnswer', event)"
      />
      {{ risposta }}
    </label>
  </div>
</template>

<script>
export default {
    name: 'Answer',

    props: ["domande"],

    data() {
    return {
      //   random: 0,
      domandaCorrente: 0,
      event: "",
    //   correct: 0,
      selected: false,
    //   finish: false,
    //   gift: [1000, 2000, 3000, 5000, 10000, 15000, 25000, 40000, 50000, 100000],
    //   finalPrice: 0,
    };
  },
};
</script>

<style>
</style>