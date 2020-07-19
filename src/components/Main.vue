<template>
  <div class="main">
    <header class="header" role="banner">
      <hgroup>
        <h1 class="text-center">MERCOSUL BOARD</h1>
        <h2 class="text-center">
          Adding mercosul plates for
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
            <SingleLine header="Marca" :content="sinestResult.marca" />
            <SingleLine header="Modelo" :content="sinestResult.modelo" />
            <SingleLine header="Ano" :content="sinestResult.ano" />
            <SingleLine header="Cor" :content="sinestResult.cor" />
            <SingleLine
              header="Localidade"
              :content="sinestResult.municipio + '/' + sinestResult.uf"
            />
          </div>
        </transition>
      </div>
      <div class="content_container">
        <form class="form" v-on:submit.prevent="formSubmit">
          <InputTxt
            label="Digite uma placa"
            :color="inputPlate.color"
            :placeholder="inputPlate.placeholder"
            :pattern="inputPlate.pattern"
            :required="true"
            v-on:keyup="getValue"
            :focusable="true"
            :tip="inputPlate.label"
          />
          <div class="input__container">
            <label for="radios_group">Tipo de veículo</label>
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
            <Button value="Salvar" :disabled="buttonDisabled" v-on:click="formSubmit" />
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
      </div>
    </main>
    <Saving :contents="plates" />
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
      board: {
        country: "Brasil",
        flag: "flagBrasil-min.png",
        letter: "ABC1D23",
        color: "#000000"
      },
      inputPlate: {
        placeholder: "ABC1D23",
        pattern: "[a-zA-Z]{3}[0-9]{1}[a-zA-Z][0-9]{2}",
        label: "Sequência alfanumérica de 7 dígitos",
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
    getValue(input) {
      let result = input.toUpperCase();
      if (result.length == 0) {
        this.board.letter = this.inputPlate.placeholder;
        this.setInput("Sequência alfanumérica de 7 dígitos", "NORMAL");
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
          this.setInput("O valor não corresponde a nenhum país", "ERROR");
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

      if (this.plates.includes(value)) {
        this.sendMessage("ERRO", "Valor repetido", "ERROR");
      } else if (value.length == 0) {
        this.sendMessage("CUIDADO", "Valor vazio", "ALERT");
      } else {
        this.plates.push(value);
        this.sendMessage("Concluído", "Valor salvo com sucesso", "SUCESSFUL");
        this.setInput("Sequência alfanumérica de 7 dígitos", "NORMAL");
        this.$emit("clean");
        this.buttonDisabled = true;
        this.getPlateInfo("", "");
      }

      console.log(this.plates);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2,
h3,
h4,
h5,
h6 {
  color: #232121;
  letter-spacing: -1px;
}

mark {
  color: #232121;
  padding: 0 5px;
  background-color: #ccb82d;
  border-radius: 2px;
  box-shadow: 2px 3px 1px #00000045;
}

.text-center {
  text-align: center;
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
