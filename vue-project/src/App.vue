<script>
import axios from 'axios'

export default {
// Переменные
// Экспорт по-умолчанию
    data() {
        return {
        city: "",
        error: "",
        info: null
        }
    },
    computed: {
        // Автоматически-обрабатываемые свойства
        cityName() {
            return "городе: " + this.city
        },

        showTemp() {
            return "Температура: " + this.info.main.temp
        },

        showTempFeelsLike() {
            return "Ощущается как: " + this.info.main.feels_like
        },

        showMinTemp() {
            return "Минимальная температура: " + this.info.main.temp_min
        },

        showMaxTemp() {
            return "Максимальная температура: " + this.info.main.temp_max
        }

    },
    methods: {
        getWeather() {
            if(this.city.trim().length < 2) {
                this.error = "Небходимо название более одного символа"
                return false
            }
            this.error = ''

            axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=1c897120d269f53fde991af4ba11c7ad`)
                .then(res => (this.info = res.data))
        }
    }
}
</script>

<template>
    <div class="wrapper">

        <h1>Погодное приложение</h1>
        <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>             <!-- • Если city == пустой строке, тогда вывести в "вашем городе", иначе вывести то, что будет внутри этого свойства -->
        <input type="text" v-model="city" placeholder="Название города:">               <!-- • ОБРАБОТЧИК СОБЫТИЯ v-model. Вводимые данные от пользователя будут заноситься в переменную city.
                                                                                        Двусторонняя привязка: v-model  синхронизирует данные между свойством компонента и значением
                                                                                        элемента ввода (input). Изменения в элементе ввода автоматически обновляют свойство, и наоборот.   -->
        <button v-if="city != ''" @click="getWeather()">Получить погоду</button>                              <!-- • Если city не пусто, то вывести кнопку "Получить погоду" -->
        <button disabled v-else>Введите название города</button>                        <!-- • Иначе вывести некликабельную кнопку "Введите название города" -->
        <p class="error">{{ error }}</p>

        <img src="./assets/icons/logo.png" alt="Иконка погоды" class="weather-icon" width="80" height="80">

        <div v-if="info != null">
            <p>{{ showTemp }}</p>
            <p>{{ showTempFeelsLike }}</p>
            <p>{{ showMinTemp }}</p>
            <p>{{ showMaxTemp }}</p>
        </div>
    </div>
</template>


<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Play:wght@400;700&display=swap');

.error {
    color: rgb(210, 6, 6);
}

.play-regular {
  font-family: "Play", sans-serif;
  font-weight: 400;
  font-style: normal;
}

.play-bold {
  font-family: "Play", sans-serif;
  font-weight: 700;
  font-style: normal;
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 40px;
  background: rgb(128,72,217);
  background: linear-gradient(165deg, rgba(128,72,217,1) 0%, rgba(10,26,78,1) 100%);
  text-align: center;
  color: white;
}

.wrapper h1 {
  margin-top: 50px;
  font-family: "Play", sans-serif;
}

.wrapper p {
  margin-top: 20px;
  font-family: "Play", sans-serif;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid rgba(209, 19, 123, 0.578);
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: rgb(209, 146, 57);
}

.wrapper button:disabled {
  font-family: "Play", sans-serif;
  background: #b4701e;
  cursor: not-allowed;
}

.wrapper button {
  font-family: "Play", sans-serif;
  background: #dd8b26;
  color: #fff;
  border-radius: 10px;
  border: 2px solid rgba(209, 19, 123, 0.578);
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover:enabled {
  transform: scale(1.1) translateY(-5px);
}

.wrapper input {
  font-family: "Play", sans-serif;
}

</style>
