<template>
  <div class="board" v-bind:style="{borderColor: color }">
    <div class="first_part">
      <div class="mercosul"></div>
      <div class="country">{{country}}</div>
      <div class="flag">
        <img :src="require('../assets/' + flag)" alt="Flag" />
      </div>
    </div>
    <div class="board_letter">
      <span class="board_letterText" v-bind:style="{color: color }">{{letter}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Board",
  props: {
    country: String,
    letter: String,
    color: String,
    flag: String
  }
};
</script>

<style scoped>
@font-face {
  font-family: "Reg Plate"; /*a name to be used later*/
  src: url("../assets/reg_plate_UK.ttf"); /*URL to font*/
}

.board {
  font-family: "Reg Plate";
  box-shadow: inset 0px 0px 1px 3px #00000045, 0px 0px 0 1px #4a4848,
    0px 0px 0 2px #151313;
  background-color: #bec3c6;
  /*padding: 12px;*/
  font-weight: bolder;
  height: 117px;
  overflow: hidden;
  width: 360px;
  cursor: default;
  /*filter: saturate(0.85);*/
  border: 2px solid #000000d6;
  text-align: center;
  margin: 0 auto;
  transition: color 0.5s ease-in-out;
  border-radius: 8px;
}

.board:hover {
  filter: drop-shadow(1px 3px 3px black);
}

.board .first_part {
  background-color: #0038a8;
  background-image: linear-gradient(to right, #e6e6e6 50%, transparent 50%),
    linear-gradient(to left, #e6e6e6 50%, transparent 50%);
  background-position: 25% 12%, 75% 12%;
  background-size: 35px 4px;
  background-repeat: no-repeat;
  color: #fff;
  z-index: 2;
  letter-spacing: 2px;
  font-weight: 100;
  padding: 5px 0 0px 0;
  display: flex;
  justify-content: space-between;
  font-variant: all-small-caps;
  /*text-shadow: 0px 1px 1px black;*/
}

.board .first_part .mercosul,
.board .first_part .flag {
  background-image: none;
  width: 25px;
  margin: 2px 10px;
  background-position: center;
  background-repeat: no-repeat;
}
.board .first_part .mercosul {
  background-size: contain;
  background-image: url("../assets/logoMercosul-min.png");
  filter: invert(1);
}

.board .first_part .flag {
  background-size: 100% 100%;
  height: 17px;
  border-radius: 4px;
  overflow: hidden;
  box-shadow: 0px 0px 0px 1px #ffffff;
}

.board .first_part .flag img {
  width: 100%;
  height: 100%;
}

.board .board_letter {
  font-size: 3.5em;
  color: #000;
  letter-spacing: 9px;
  padding-left: 10px;
  padding-top: 8px;
  font-weight: bold;
  text-shadow: 3px 0px 0px #a6a6a9, -3px 0px 2px #dacece;
  font-stretch: extra-expanded;
  background-image: linear-gradient(to right, #e6e6e6 50%, transparent 50%),
    linear-gradient(to left, #e6e6e6 50%, transparent 50%);
  background-position: 25% 88%, 75% 88%;
  background-size: 35px 4px;
  background-repeat: no-repeat;
}

.board_letterText {
  background: url("../assets/signMercosul-min.png") center;
  background-size: 10px 10px;
  background-clip: text;
  transition: all 0.5s ease-in-out;
  color: #000;
}

@media print {
  .board > .first_part {
    filter: invert(100%);
    color: #fff;
  }
  .board .first_part .mercosul {
    background-size: contain;
    background-image: url("../assets/logoMercosul-min.png");
    filter: invert(10%);
  }

  .board .first_part .flag {
    filter: invert(100%);
  }

  .board .first_part .country {
    filter: invert(50%);
  }
}
</style>