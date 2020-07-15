<template>
  <div class="main">
    <h1>MERCOSUL BOARD</h1>
    <div class="content">
      <div class="content_container">
        <Board
          :country="board.country"
          :letter="board.letter"
          :color="board.color"
          :flag="board.flag"
        />
        <transition name="slide-fade" mode="in-out">
          <div class="infos" v-if="Object.keys(sinestResult).length > 0">
            <SingleLine header="Marca" :content="sinestResult.marca" />
            <SingleLine header="Modelo" :content="sinestResult.modelo" />
            <SingleLine header="Ano" :content="sinestResult.ano" />
            <SingleLine header="Cor" :content="sinestResult.cor" />
            <SingleLine header="Município" :content="sinestResult.municipio" />
            <SingleLine header="Estado" :content="sinestResult.uf" />
          </div>
        </transition>
      </div>
      <div class="content_container">
        <form class="form">
          <InputTxt
            :label="inputPlate.label"
            :color="inputPlate.color"
            :placeholder="inputPlate.placeholder"
            :pattern="inputPlate.pattern"
            :required="true"
            v-on:keyup="getInputValue"
            @submitInput="submit"
            :focusable="true"
            tip="Sequência alfanumérica de 7 digítos"
          />
          <div class="input__container">
            <label for="radios_group">Tipo de veículo</label>
            <div name="radios_group" class="radios_group">
              <Radio
                v-for="(radio, values) in radios"
                :key="(radio, values)"
                :name="radio.name"
                :value="radio.value"
                :checked="radio.checked"
                group="cartype"
                v-on:click="setPlateColor"
              />
            </div>
          </div>
          <div class="input__container">
            <Button value="Adicionar" :disabled="buttonDisabled" v-on:click="submit" />
          </div>
          <transition name="slide-fade" mode="in-out">
            <Message
              v-if="message.show"
              :title="message.title"
              :color="message.color"
              :text="message.text"
            />
          </transition>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import Board from "./Board";
import Radio from "./Radio";
import SingleLine from "./SingleLine";
import Message from "./Message";
import Button from "./Button";
import InputTxt from "./InputTxt";

//jsons
import Countries from "../data/Countries.json";
import Colors from "../data/Colors.json";

//api do sinest
import Sinest from "sinesp-api";

export default {
  name: "Main",
  components: {
    Board,
    InputTxt,
    Radio,
    SingleLine,
    Message,
    Button
  },
  data() {
    return {
      board: {
        country: "Brasil",
        flag: "flagBrasil-min.png",
        letter: "ABC1D23",
        color: "#000000"
      },
      inputPlate: {
        placeholder: "ABC1D23",
        pattern: "^[a-zA-Z]{3}[0-9]{1}[a-zA-Z][0-9]{2}$",
        label: "Digite uma placa",
        color: "#000"
      },
      buttonDisabled: true,
      message: {
        title: "",
        color: "",
        text: "",
        show: false
      },
      plates: [],
      radios: [
        { name: "Particular", value: "#000000", checked: true },
        { name: "Aluguel e auto-escola", value: "#c8102e", checked: false },
        { name: "Administração publica", value: "#0033a0", checked: false },
        { name: "Diplomático", value: "#f2a900", checked: false },
        { name: "Especial", value: "#007a53", checked: false },
        { name: "Coleção", value: "#968f8b", checked: false }
      ],
      sinestResult: {}
    };
  },
  methods: {
    getInputValue(inputValue) {
      let result = inputValue.toUpperCase();
      if (result.length == 0) {
        this.board.letter = this.inputPlate.placeholder;
        this.setInput("Digite uma placa", "NORMAL");
      } else {
        if (JSON.stringify(this.getCountry(result)) != JSON.stringify({})) {
          let country = this.getCountry(result);
          this.board.letter = result;

          this.setInput("Formato reconhecido", "SUCESSFUL");
          this.setBoard(country);
          this.buttonDisabled = result.length < 7;
          this.getPlateInfo(country.country, result);
        } else {
          this.buttonDisabled = true;
          this.setInput("Valor incorreto", "ERROR");
        }
      }
    },
    setPlateColor(value) {
      this.board.color = value;
    },
    getCountry(plate) {
      let result = {};
      Countries.forEach(country => {
        if (plate.length < 7) {
          if (plate.match(country.inicial)) result = country;
        } else {
          if (plate.match(country.validator)) result = country;
        }
      });
      return result;
    },
    setBoard(result) {
      this.board.country = result.country;
      this.board.flag = result.flagUrl;
      this.inputPlate.pattern = result.validator;
      this.inputPlate.placeholder = result.placeholder;
    },
    sendMessage(title = "", text, status = "INFORMATION") {
      this.message.title = title;
      this.message.text = text;
      this.message.color = Colors[status];

      this.message.show = true;
      setTimeout(() => {
        this.message.show = false;
      }, 2000);
    },
    setInput(label, status = "NORMAL") {
      this.inputPlate.label = label;
      this.inputPlate.color = Colors[status];
    },
    async getPlateInfo(country, plate) {
      if (country == "Brasil" && plate.length == 7) {
        await Sinest.search(plate)
          .then(value => {
            this.sinestResult = value;
          })
          .catch(err => {
            this.sendMessage("ERRO", err, "ERROR");
          });
      } else {
        this.sinestResult = {};
      }
    },
    submit() {
      //console.log(value);
      if (this.plates.includes(this.board.letter)) {
        this.sendMessage("ERRO", "Valor repetido", "ERROR");
      } else {
        this.plates.push(this.board.letter);
        this.sendMessage(
          "Concluído",
          "Valor adicionado com sucesso",
          "SUCESSFUL"
        );
      }
      console.log(this.plates);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@font-face {
  font-family: "Reg Plate"; /*a name to be used later*/
  src: url("../assets/reg_plate_UK.ttf"); /*URL to font*/
}

h1 {
  font-family: Reg Plate;
  text-align: center;
  opacity: 0.5;
}

.content {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-around;
}

.content_container {
  margin: 30px;
}

.infos {
  padding: 10px 0;
}

.form {
  display: flex;
  flex-flow: column;
}

.radios_group {
  margin-top: 10px;
  display: flex;
  flex-flow: row wrap;
}

.input__container {
  padding: 4px;
  margin-top: 10px;
  display: flex;
  flex-flow: column nowrap;
}

/** miscelania */

.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
  opacity: 0;
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>
