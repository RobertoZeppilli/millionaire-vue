<template>
  <div id="app">
    <div class="logo">
      <img src="./assets/logo.png" alt="" />
    </div>
    <Question />
  </div>
</template>

<script>
import Question from "./components/Question.vue";

export default {
  name: "App",

  components: {
    Question,
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.logo {
  height: 30vh;
  background-color: #0e0e88;
  padding: 30px 0;
  display: flex;
  justify-content: center;
  align-items: center;

  img {
    // width: 100%;
    height: 150px;
  }
}

.question {
  height: 70vh;
  background-color: #11093a;
  color: #fff;
}

.hidden {
  display: none;
}

.border {
  text-align: center;
  border: 1px solid #fff;
  position: relative;
  padding: 20px 60px;
  
}

.q {
  padding: 30px 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
  position: relative;
}

.hover {
  transition: all 0.3s ease-in-out;
  &:hover {
    // background-color: lightgrey;
    // color: #fff;
    border: 1px solid gold;
    cursor: pointer;
  }
}

button {
  position: absolute;
  bottom: -50px;
  right: 0px;
  padding: 10px 20px;
  background-color: transparent;
  cursor: pointer;
  color: #fff;
  font-weight: bold;

  border: none;
  outline: none;
  transition: all 0.4s ease-in-out;
  &:hover {
    color: gold;
  }
}

.bg-red {
  background-color: rgb(245, 128, 128);
  border: 1px solid red;
  color: #fff;
}

.bg-green {
  background-color: rgb(151, 224, 151);
  border: 1px solid green;
  color: #fff;
}

.answer {
  display: flex;
  flex-wrap: wrap;
  position: relative;
  // background-color: yellow;
  // height: 100%;
  width: 80%;
  margin-top: 30px;
}

.answer-label {
  // border: 1px solid #fff;
  position: relative;
  width: calc(100% / 2 - 2rem);
  padding: 10px;
  margin: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
}

.index {
  position: absolute;
  top: 50%;
  left: 10px;
  transform: translateY(-50%);
  color: gold;
}
</style>


