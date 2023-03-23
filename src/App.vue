<!-- Документация к Vue 3: https://v3.ru.vuejs.org/  -->
<!-- Документация к GSAP: https://greensock.com/docs/ -->

<!-- Верстка приложения -->
<template>
  <!-- Хедер -->
  <header>
    <h3 ref="logo">R4ndw0rd</h3>
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
        <h5>User Settings</h5>
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
    // GSAP анимация логотипа
    const logo = this.$refs.logo;

    gsap.from(logo, {
      duration: 1,
      opacity: 0,
      x: -50,
      ease: "power2.out",
    });
  },
};
</script>

<style lang="scss">
// Импорт стилизованных шрифтов
@import url(./assets/styles/fonts/fonts.css);

// Импорт основных стилей
@import url(./assets/styles/main/main.css);
</style>
