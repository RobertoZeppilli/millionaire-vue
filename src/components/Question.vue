<template>
  <div class="question">
    <div class="q" v-if="domandaCorrente < domande.length && !finish">
      <h2 class="border">{{ domande[domandaCorrente].domanda }}</h2>

      <!-- <Answer @selectAnswer="setAnswer" :domande="domande" /> -->
      <div class="answer">
        <label
          class="answer-label border hide"
          :for="index"
          v-for="(risposta, index) in domande[domandaCorrente].risposte"
          :key="index"
          :class="{
            hover: domandaScelta == '',
            'bg-red': domandaScelta == index,
            'bg-green':
              index == domande[domandaCorrente].rispostaCorretta &&
              domandaScelta != '',
          }"
        >
          <input
            type="radio"
            :checked="index == selected"
            :name="index"
            :id="index"
            class="hidden"
            :value="index"
            @change="select($event)"
          />

          <h4 class="index">{{ index }}</h4>
          <h4 class="actual-answer">{{ risposta }}</h4>
        </label>
        <button
          class="half"
          :disabled="deletedAnswers"
          :class="{ hidden: deletedAnswers || selected }"
          @click="getHalf(domande[domandaCorrente])"
          @mouseover="title = true"
          @mouseleave="title = false"
        >
          <span>50 : 50</span>
        </button>
        <div class="title" v-show="title">
          <p>Puoi usarlo una sola volta, Scegli con cura!</p>
        </div>
        <div
          v-show="
            domandaScelta != '' &&
            domandaCorrente < domande.length - 1 &&
            !finish
          "
          class="button"
        >
          <button v-show="!endGame" @click="nextQuestion()">
            Vai alla domanda da {{ nextPrice | toCurrency }}
          </button>
          <h4 class="wrong" v-show="endGame">Hai Sbagliato!</h4>
        </div>
        <div
          v-show="
            domandaScelta != '' &&
            domandaCorrente == domande.length - 1 &&
            !finish
          "
          class="button"
        >
          <button @click="showResult()">Finish</button>
        </div>
      </div>

      <div class="ladder">
        <h6
          :class="{ gold: domandaCorrente == index }"
          v-for="(price, index) in gift"
          :key="index"
        >
          {{ price | toCurrency }}
        </h6>
      </div>
    </div>
    <div v-else-if="finish || domandaCorrente == domande.length">
      <div class="end">
        <!-- <h2>
          Correct Answers: <span class="t-green">{{ correct }}</span>
        </h2> -->
        <h2 v-show="domandaCorrente != domande.length">
          Ti sei fermato alla domanda {{ domandaCorrente + 1 }}
        </h2>
        <div class="custom" v-show="domandaCorrente == domande.length">
          <h2 style="text-align: center; font-size: 40px; color: gold">
            Complimenti!
          </h2>
          <img style="width: 150px" src="../assets/money-cash.gif" alt="" />
        </div>
        <h5 :class="{ 'm-top': domandaCorrente < domande.length }">
          Torni a casa con
        </h5>
        <h2 style="color: gold; font-size: 3rem">
          {{ finalPrice | toCurrency }}
        </h2>
        <button class="reload" @click="reloadQuiz()">
          Inizia di nuovo la scalata!
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
export default {
  name: "Question",
  data() {
    return {
      domandaCorrente: 0,
      title: false,
      correct: 0,
      nextPrice: 0,
      endGame: false,
      domandaScelta: "",
      deletedAnswers: false,
      selected: false,
      finish: false,
      gift: [
        1000, 2000, 3000, 5000, 10000, 15000, 25000, 40000, 50000, 100000,
        200000, 500000, 1000000,
      ],
      finalPrice: 0,
      domande: [
        {
          domanda: "Quanti anni può vivere, al massimo, un’oca?",
          risposte: { a: "2 anni", b: "5 anni", c: "40 anni", d: "10 anni" },
          rispostaCorretta: "c",
        },
        {
          domanda: "La capitale della Romania?",
          risposte: { a: "Bucarest", b: "Milano", c: "Berlino", d: "Oslo" },
          rispostaCorretta: "a",
        },
        {
          domanda: "Quanto misura il raggio terrestre?",
          risposte: {
            a: "6368 km",
            b: "6234 km",
            c: "4356 km",
            d: "10.234 km",
          },
          rispostaCorretta: "a",
        },
        {
          domanda: "Quando è iniziata la prima guerra mondiale?",
          risposte: { a: "1993", b: "1910", c: "1915", d: "1879" },
          rispostaCorretta: "c",
        },
        {
          domanda: "Dove fu costruita la prima linea ferroviaria?",
          risposte: {
            a: "In Germania nel 1834",
            b: "In Norvegia nel 1799",
            c: "In Italia nel 1802",
            d: "In Scozia nel 1825",
          },
          rispostaCorretta: "d",
        },
        {
          domanda:
            "In quale lingua fu scritto il Milione, resoconto del viaggio in Asia di Marco Polo? ",
          risposte: { a: "Slavo", b: "Tedesco", c: "Italiano", d: "Francese" },
          rispostaCorretta: "d",
        },
        {
          domanda:
            "Quali tra i seguenti alimenti non era conosciuto dai romani?",
          risposte: { a: "Miele", b: "Melone", c: "Pomodoro", d: "Pane" },
          rispostaCorretta: "c",
        },
        {
          domanda: "Quali tra i seguenti stati è situato a nord dell’equatore?",
          risposte: {
            a: "Australia",
            b: "Thailandia",
            c: "Perù",
            d: "Tanzania",
          },
          rispostaCorretta: "b",
        },
        {
          domanda:
            "Quante pulsazioni compie il cuore umano di un adulto, all’incirca, in un anno?",
          risposte: {
            a: "3 milioni",
            b: "1 milione",
            c: "10 milioni",
            d: "4 milioni",
          },
          rispostaCorretta: "a",
        },
        {
          domanda: "Quale pianeta del sistema solare ha 5 satelliti?",
          risposte: { a: "Urano", b: "Venere", c: "Saturno", d: "Giove" },
          rispostaCorretta: "a",
        },
        {
          domanda: "Come si distingue un elefante asiatico da uno africano?",
          risposte: {
            a: "dalla grandezza delle orecchie",
            b: "dal colore",
            c: "dalla lunghezza della coda",
            d: "dal peso",
          },
          rispostaCorretta: "a",
        },
        {
          domanda: "Da un albero quanti fogli si ricavano?",
          risposte: {
            a: "circa 1.800",
            b: "circa 48.000",
            c: "circa 79.000",
            d: "circa 30.000",
          },
          rispostaCorretta: "c",
        },
        {
          domanda: "Dove si propaga più velocemente il suono?",
          risposte: {
            a: "Nello spazio",
            b: "Sotto terra",
            c: "Nell'aria",
            d: "Nell'acqua",
          },
          rispostaCorretta: "d",
        },
      ],
    };
  },

  methods: {
    select(e) {
      this.selected = e.target.value;
      this.domandaScelta = e.target.value;
      //   console.log(this.domandaScelta);
      //   console.log(this.domandaCorrente);
      if (
        this.domandaScelta ==
        this.domande[this.domandaCorrente].rispostaCorretta
      ) {
        this.correct++;
        this.finalPrice = this.gift[this.domandaCorrente];
        this.nextPrice = this.gift[this.domandaCorrente + 1];
        this.finish = false;
      } else {
        // this.reload();
        this.endGame = true;
        setTimeout(() => {
          this.finish = true;
        }, 1500);
        // this.wrong++
      }
    },

    shuffleVueArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        const temp = array[i];
        Vue.set(array, i, array[j]);
        Vue.set(array, j, temp);
      }
    },

    nextQuestion() {
      this.domandaScelta = "";

      this.selected = false;
      this.domandaCorrente++;
    },

    showResult() {
      this.domandaCorrente++;
    },

    reloadQuiz() {
      location.reload();
    },

    toCurrency() {
      Vue.filter("toCurrency", function (value) {
        if (typeof value !== "number") {
          return value;
        }
        var formatter = new Intl.NumberFormat("en-US", {
          style: "currency",
          currency: "USD",
          minimumFractionDigits: 0,
        });
        return formatter.format(value);
      });
    },

    getHalf(obj) {
      let nr = 0;
      function removeProperty(propKey, { [propKey]: propValue, ...rest }) {
        console.log(propValue);
        return rest;
      }
      for (var k in obj.risposte) {
        if (nr < 2) {
          if (k != obj.rispostaCorretta) {
            obj.risposte = removeProperty(k, obj.risposte);
            this.deletedAnswers = true;
            nr++;
          }
        } else return;
      }
    },
  },

  created() {
    this.shuffleVueArray(this.domande);
    this.toCurrency();
  },
};
</script>

<style lang="scss">
.end {
  position: relative;
  height: 70vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  .reload {
    position: absolute;
    bottom: 50px;
    left: 50%;
    transform: translateX(-50%);
    border: 1px solid #fff;
    padding: 10px 20px;
    transition: all 0.3s ease-in-out;

    &:hover {
      border: 1px solid gold;
      color: #fff;
    }
  }
}

.m-top {
  margin-top: 20px;
}
.gold {
  // background-color: gold;
  border: 1px solid gold;
  color: #fff;
}
.ladder {
  position: absolute;
  left: 10px;
  top: 50%;
  transform: translateY(-50%);

  h6 {
    text-align: center;
    margin-bottom: 5px;

    padding: 5px;
    border-radius: 20px;
  }
}
.wrong {
  color: rgb(245, 128, 128);
  font-size: 2rem;
  position: absolute;
  bottom: -70px;
  left: 50%;
  transform: translateX(-50%);
}
.title {
  font-size: 12px;
  position: absolute;
  left: 0;
  bottom: -20px;
  margin-left: 1rem;
  color: gold;
}
.half {
  position: absolute;
  left: 50%;
  bottom: -50px;
  transform: translateX(-50%);
  border: 5px solid #0e0e88;
  background: #373b44; /* fallback for old browsers */
  background: -webkit-linear-gradient(
    to right,
    #4286f4,
    #373b44
  ); /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(
    to right,
    #4286f4,
    #373b44
  ); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */

  border-radius: 40px;
  transition: all 0.5s ease-in-out;
  &:hover {
    color: #fff;
    border-color: gold;
  }
}

.custom {
  margin-bottom: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>