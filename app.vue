<!-- <template>
  <div>
    <div id="google_translate_element"></div>
    <h1>Hello World! Good Morning</h1>
  </div>
</template>

<script>
export default {
  mounted() {
    this.removeGoogleTranslateBanner();
    this.loadGoogleTranslateScript();
  },
  methods: {
    loadGoogleTranslateScript() {
      const script = document.createElement('script');
      script.type = 'text/javascript';
      script.src = 'https://translate.google.com/translate_a/element.js?cb=googleTranslateElementInit';
      script.async = true;
      document.head.appendChild(script);

      window.googleTranslateElementInit = () => {
        new window.google.translate.TranslateElement({
          pageLanguage: 'en',
          includedLanguages: 'en,es,fr,de,it,pt',
          layout: window.google.translate.TranslateElement.InlineLayout.SIMPLE,
        }, 'google_translate_element');
      };
    },
    removeGoogleTranslateBanner() {
      setInterval(() => {
        const bannerFrame = document.querySelector('.goog-te-banner-frame');
        if (bannerFrame) {
          bannerFrame.style.display = 'none';
        }

        const body = document.body;
        if (body.style.top) {
          body.style.top = '0px';
        }

        document.querySelectorAll('iframe').forEach((iframe) => {
          if (iframe.src.includes('translate.google.com')) {
            iframe.remove();
          }
        });
      }, 500);
    },
  },
};
</script>

<style scoped>
.goog-te-banner-frame {
  display: none !important;
}
.goog-te-balloon-frame {
  display: none !important;
}
body {
  top: 0px !important;
}
</style> -->

















<template>
  <div>
    <select id="languageSwitcher" v-model="currentLanguage" @change="changeLanguage">
      <option value="en">English</option>
      <option value="es">Spanish</option>
      <option value="fr">French</option>
      <option value="de">German</option>
      <option value="it">Italian</option>
      <option value="pt">Portuguese</option>
      <option value="hi">Hindi</option>
    </select>

    <h1>{{ welcomeText }}</h1>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import i18next from 'i18next';
import LanguageDetector from 'i18next-browser-languagedetector';

// Define the resources for translation
const resources = {
  en: { translation: { welcome: 'Welcome to Sekel Tech Pvt Ltd!' } },
  es: { translation: { welcome: '¡Bienvenido a Sekel Tech Pvt Ltd!' } },
  fr: { translation: { welcome: 'Bienvenue chez Sekel Tech Pvt Ltd!' } },
  de: { translation: { welcome: 'Willkommen bei Sekel Tech Pvt Ltd!' } },
  it: { translation: { welcome: 'Benvenuto in Sekel Tech Pvt Ltd!' } },
  pt: { translation: { welcome: 'Bem-vindo à Sekel Tech Pvt Ltd!' } },
  hi: { translation: { welcome: 'सेकेल टेक प्राइवेट लिमिटेड में आपका स्वागत है!' } },
};

// Define the language model and the text to display
const currentLanguage = ref('hi');
const welcomeText = ref('');

// Initialize i18next with the language detector and resources
const initI18Next = () => {
  i18next
    .use(LanguageDetector)
    .init(
      {
        lng: currentLanguage.value,
        fallbackLng: 'hi',
        resources,
      },
      (err, t) => {
        if (!err) {
          welcomeText.value = t('welcome');
        }
      }
    );
};

// Function to change language
const changeLanguage = () => {
  i18next.changeLanguage(currentLanguage.value, (err, t) => {
    if (!err) {
      welcomeText.value = t('welcome');
    }
  });
};

// Initialize i18next and set the welcome text on mounted
onMounted(() => {
  initI18Next();
});
</script>

<style scoped>
select {
  margin: 20px 0;
}
</style>













