<!-- Документация к Vue 3: https://v3.ru.vuejs.org/  -->
<!-- Документация к GSAP: https://greensock.com/docs/ -->

<!-- Верстка приложения -->
<template>
  <!-- Хедер -->
  <header>
    <h3>R4ndw0rd</h3>
  </header>
  <!-- Корневой узел Vue -->
  <div id="app">
    <!-- Сетка приложения -->
    <div class="wrapper">
      <!-- Заголовок -->
      <h1 ref="title">Create strong passwords quickly and easily</h1>
      <!-- Инпут для генерации пароля -->
      <section class="password-box">
        <input type="text" class="password-input" v-model="password" readonly />
        <img
          src="./assets/icons/copy.png"
          @click="copyPassword"
          class="password-img scale-up"
          alt=""
        />
      </section>
      <!-- Кнопка, вызывающая по клику генерацию пароля -->
      <button class="create-btn" @click="generatePassword">
        Generate Password
      </button>
      <!-- Чекбар для настройки характеристик генерируемого пароля -->
      <section class="check-bar">
        <h4>User Settings</h4>
        <!-- Инпут для регулирования длинны пароля -->
        <div class="checkbox">
          <input type="number" v-model.number="length" min="4" max="32" />
          <label><h6>Password Length</h6></label>
        </div>
        <!-- Чекбокс для генерации заглавных символов в пароле -->
        <div class="checkbox">
          <input type="checkbox" id="uppercase" v-model="uppercase" />
          <label for="uppercase"><h6>Include Uppercase</h6></label>
        </div>
        <!-- Чекбокс для генерации специальных символов в пароле  -->
        <div class="checkbox">
          <input type="checkbox" id="specialChars" v-model="specialChars" />
          <label for="specialChars"><h6>Include Special Characters</h6></label>
        </div>
        <!-- Чекбокс для регулировки длинны пароля-->
        <div class="checkbox">
          <input type="checkbox" id="includeNumbers" v-model="numbers" />
          <label for="includeNumbers"><h6>Include Numbers</h6></label>
        </div>
      </section>
    </div>
  </div>
  <!-- Футер -->
  <footer>
    <h5>Contacts</h5>
    <a target="_blank" href="https://t.me/pokaneprishel"><h6>Telegram</h6></a>
    <a target="_blank" href="https://github.com/pokaneprishel"
      ><h6>GitHub</h6></a
    >
  </footer>
</template>

<script>
import { gsap } from "gsap";

export default {
  name: "App",

  data() {
    return {
      length: 8,
      uppercase: false,
      specialChars: false,
      numbers: false,
      password: "",
      inputText: "",
    };
  },
  methods: {
    toggleNumbers() {
      this.numbers = !this.numbers;
    },
    // Метод генерирующий пароль
    generatePassword() {
      // Поля для генерации пароля
      const lowercaseChars = "abcdefghijklmnopqrstuvwxyz";
      const uppercaseChars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
      const specialChars = "!@#$%^&*()_+~`|}{[];?><,./-=";
      const numberChars = "0123456789";

      // Ветки, передающие в верстку настройки генерации пароля
      let chars = lowercaseChars;
      if (this.uppercase) chars += uppercaseChars;
      if (this.specialChars) chars += specialChars;
      if (this.numbers) chars += numberChars;

      // Метод генерации длинны пароля
      let password = "";
      for (let i = 0; i < this.length; i++) {
        password += chars.charAt(Math.floor(Math.random() * chars.length));
      }

      // Передача метода директиве
      this.password = password;
    },
    // Копирование сгенерированной в инпуте строки
    copyPassword() {
      const passwordInput = document.querySelector(".password-input");
      passwordInput.select();
      document.execCommand("copy");
    },
  },
  // Хуки жизненного цикла
  mounted() {
    // Рендер анимацию 'печатания' заголовка с помошью библиотеки GSAP
    const title = this.$refs.title;
    const chars = title.textContent.split("");

    title.textContent = "";

    chars.forEach(function (char) {
      const span = document.createElement("span");
      span.textContent = char;
      title.appendChild(span);
    });

    gsap.fromTo(
      title.children,
      { opacity: 0, x: -20 },
      {
        duration: 0.5,
        opacity: 1,
        x: 0,
        stagger: 0.05,
        ease: "power2.out",
      }
    );
  },
};
</script>

<style lang="scss">
// Импорт стилизованных шрифтов
@import url(./assets/styles/fonts/fonts.scss);

// Глобальные стилизация
#app {
  min-height: 100vh;
  background: linear-gradient(90deg, #567599, #1f2e4b);
  font-family: "Ubuntu Mono", monospace;
}

// Глобальная сетка приложения
.wrapper {
  display: grid;
  grid-template-columns: repeat(1, minmax(375px, 800px));
  grid-template-rows: 25vh 18vh 6vh 50vh;
  justify-content: center;
  align-items: center;
  justify-items: center;

  h1 {
    text-align: center;
  }
}

// Cтилизация кнопки
.create-btn {
  width: 15rem;
  height: 3rem;

  background: white;

  -webkit-box-shadow: 0px 0px 20px 5px rgba(255, 255, 255, 0.2);
  -moz-box-shadow: 0px 0px 20px 5px rgba(255, 255, 255, 0.2);
  box-shadow: 0px 0px 20px 5px rgba(255, 255, 255, 0.2);

  border: none;
  border-radius: 20px;
  cursor: pointer;

  font-size: 1rem;
  font-weight: 600;
  padding: 10px 20px;
  text-transform: uppercase;
  color: black;
}

// Стилизация иппута
.password-input {
  width: 40rem;
  height: 100px;
  background: white;

  -webkit-box-shadow: 0px 0px 20px 5px rgba(255, 255, 255, 0.2);
  -moz-box-shadow: 0px 0px 20px 5px rgba(255, 255, 255, 0.2);
  box-shadow: 0px 0px 20px 5px rgba(255, 255, 255, 0.2);

  border-radius: 20px;
  outline: none;
  border: none;

  color: black;

  font-size: 2rem;
  font-weight: 600;
  padding: 30px;
}

// Сетка и стилизация чек-бара
.check-bar {
  display: grid;
  grid-template-columns: repeat(1, max-content);
  grid-template-rows: repeat(4, max-content);
  row-gap: 2vh;
  align-items: center;
  justify-items: start;

  padding: 30px;
  border-radius: 20px;
}

// Стилизация чекбокс-элементов
.checkbox {
  display: grid;
  grid-auto-flow: column;
  align-items: center;
  justify-items: center;

  background-color: white;
  border-radius: 20px;
  border: none;

  width: max-content;
  height: max-content;
  padding: 12px;

  -webkit-box-shadow: 0px 0px 20px 5px rgba(255, 255, 255, 0.2);
  -moz-box-shadow: 0px 0px 20px 5px rgba(255, 255, 255, 0.2);
  box-shadow: 0px 0px 20px 5px rgba(255, 255, 255, 0.2);

  h6 {
    color: black;
    font-size: 0.8rem;
    margin-left: 7px;
    margin-right: 7px;
  }

  input[type="checkbox"] {
    border-radius: 5px;
    border: 1px #8a9597 solid;
  }

  input[type="checkbox"]:before {
    background: linear-gradient(90deg, #567599, #1f2e4b);
  }

  input[type="number"] {
    border-radius: 5px;
    border: 1px #8a9597 solid;
    width: 50px;
    height: 3vh;
    padding: 3px;
  }
}

// Сетка секции инпута
.password-box {
  display: grid;
  grid-auto-flow: column;
  align-items: center;
  justify-items: center;
  column-gap: 2vh;

  img {
    max-width: 2rem;
    height: auto;
  }

  .scale-up {
    transition: transform 0.3s ease-in-out;
  }

  .scale-up:hover,
  .scale-up:focus {
    transform: scale(1.2);
  }
}

// Стили для хедера
header {
  display: grid;
  grid-auto-flow: column;
  align-items: center;
  justify-items: center;

  background: black;
}

// Стили для футера
footer {
  display: grid;
  grid-auto-flow: row;
  align-items: center;
  justify-items: start;
  row-gap: 10px;

  background: black;

  padding-top: 3vh;
  padding-bottom: 3vh;
  padding-left: 2vw;

  h6 {
    text-decoration: underline;
  }
}

// Адаптив для планшетов
@media screen and (max-width: 992px) {
  .password-input {
    width: 70vw;
    height: 8vh;
  }

  h1 {
    font-size: 2.4rem;
    padding: 10px;
  }
}

// Адаптив для мобильных устройств
@media screen and (max-width: 510px) {
  h1 {
    font-size: 2.1rem;
  }
}


</style>
