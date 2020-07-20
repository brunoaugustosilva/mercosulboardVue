<template>
  <div class="main">
    <header class="header" role="banner">
      <hgroup>
        <h1 class="text-center">MERCOSUL BOARD</h1>
        <h2 class="text-center">
          Add mercosul plates for
          <mark>your country!</mark>
        </h2>
      </hgroup>
    </header>
    <main class="content">
      <div class="content_container">
        <transition name="slide-fade" mode="in-out" appear>
          <Board
            :country="board.country"
            :letter="board.letter"
            :color="board.color"
            :flag="board.flag"
          />
        </transition>
        <transition name="slide-fade" mode="in-out">
          <div class="infos" v-if="Object.keys(sinestResult).length > 0">
            <SingleLine icon="building" header="Marca" :content="sinestResult.marca" />
            <SingleLine icon="car" header="Modelo" :content="sinestResult.modelo" />
            <SingleLine icon="calendar-alt" header="Ano" :content="sinestResult.ano" />
            <SingleLine icon="paint-brush" header="Cor" :content="sinestResult.cor" />
            <SingleLine
              icon="map-marker"
              header="Localidade"
              :content="sinestResult.municipio + '/' + sinestResult.uf"
            />
          </div>
        </transition>
      </div>
      <div class="content_container">
        <form class="form" v-on:submit.prevent="formSubmit">
          <InputTxt
            :label="strings.labels.inputTxt"
            :color="inputPlate.color"
            :placeholder="inputPlate.placeholder"
            :pattern="inputPlate.pattern"
            :required="true"
            v-on:keyup="getValue"
            :focusable="true"
            :tip="inputPlate.label"
          />
          <div class="input__container">
            <label for="radios_group">{{strings.labels.radioGroup}}</label>
            <div name="radios_group" class="radios_group" role="radiogroup">
              <Radio
                v-for="(radio, values) in radios"
                :key="(radio, values)"
                :name="radio.name"
                :value="radio.value"
                :checked="radio.checked"
                group="cartype"
                v-on:click="setPlateColor"
                v-on:keyup.enter="setPlateColor"
              />
            </div>
          </div>
          <div class="input__container">
            <Button
              :value="strings.buttons.save"
              :disabled="buttonDisabled"
              v-on:click="formSubmit"
            />
          </div>
          <transition name="slide-fade" mode="in-out">
            <Message
              v-if="message.show"
              :title="message.title"
              :color="message.color"
              :text="message.text"
              role="alert"
            />
          </transition>
        </form>
        <Saving :contents="plates" />
      </div>
    </main>
  </div>
</template>

<script>
import Board from "./Board";
import Radio from "./Radio";
import SingleLine from "./SingleLine";
import Message from "./Message";
import Button from "./Button";
import InputTxt from "./InputTxt";
import Saving from "./Saving";

//jsons
import Countries from "../data/Countries.json";
import Colors from "../data/Colors.json";
import Strings from "../data/Strings.json";

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
    Button,
    Saving
  },
  data() {
    return {
      strings: Strings["en-US"],
      board: {},
      inputPlate: {},
      buttonDisabled: true,
      message: { show: false },
      plates: [],
      radios: [],
      sinestResult: {}
    };
  },
  methods: {
    getValue(input) {
      let result = input.toUpperCase();
      if (result.length == 0) {
        this.board.letter = this.inputPlate.placeholder;
        this.setInput(this.strings.tips.Label1, "NORMAL");
        this.buttonDisabled = true;
        this.getPlateInfo("", "");
      } else {
        if (JSON.stringify(this.getCountry(result)) != JSON.stringify({})) {
          let country = this.getCountry(result);
          this.board.letter = result;

          this.setInput(this.strings.tips.Label2, "SUCESSFUL");
          this.setBoard(country);
          this.buttonDisabled = result.length < 7;
          this.getPlateInfo(country.country, result);
        } else {
          this.buttonDisabled = true;
          this.setInput(this.strings.tips.Label3, "ERROR");
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
    formSubmit() {
      let value = this.board.letter;
      const MESSAGES = this.strings.messages;
      const ERROR = MESSAGES.ERROR;
      const WARNING = MESSAGES.WARNING;
      const SUCESSFUL = MESSAGES.SUCESSFUL;

      if (this.plates.includes(value)) {
        this.sendMessage(ERROR.title, ERROR.message, "ERROR");
      } else if (value.length == 0) {
        this.sendMessage(WARNING.title, WARNING.message, "ALERT");
      } else {
        this.plates.push(value);
        this.sendMessage(SUCESSFUL.title, SUCESSFUL.message, "SUCESSFUL");
        this.setInput(this.strings.tips.Label1, "NORMAL");
        this.$emit("clean");
        this.buttonDisabled = true;
        this.getPlateInfo("", "");
      }
    }
  },
  created() {
    let strings = this.strings;
    let radios = strings.radiosLabel;

    this.radios = [
      { name: radios.Label1, value: "#000000", checked: true },
      { name: radios.Label2, value: "#c8102e", checked: false },
      { name: radios.Label3, value: "#0033a0", checked: false },
      { name: radios.Label4, value: "#f2a900", checked: false },
      { name: radios.Label5, value: "#007a53", checked: false },
      { name: radios.Label6, value: "#968f8b", checked: false }
    ];
    this.inputPlate = {
      placeholder: "ABC1D23",
      pattern: "[a-zA-Z]{3}[0-9]{1}[a-zA-Z][0-9]{2}",
      label: strings.tips.Label1,
      color: "#000"
    };
    this.board = {
      country: "Brasil",
      flag: "flagBrasil-min.png",
      letter: "ABC1D23",
      color: "#000000"
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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

@media print {
  .form {
    display: none;
  }
}

.radios_group {
  margin-top: 10px;
  display: flex;
  flex-flow: row wrap;
}

.input__container {
  padding: 4px;
  margin-top: 20px;
  display: flex;
  flex-flow: column nowrap;
}

.input__container > label {
  font-weight: 500;
  letter-spacing: -1.2px;
  margin-bottom: 4px;
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
