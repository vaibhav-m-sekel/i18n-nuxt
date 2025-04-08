<!-- FIRST EXAMPLE -->


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




<!-- SECOND EXAMPLE -->

<!-- 
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
</style> -->



<!-- THIRD EXAMPLE -->
<template>
  <div>
    <div id="google_translate_element"></div>
    <container>
      <section id="what-is-sekel-tech">
        <h2>What is Sekel Tech?</h2>
        <p>Sekel Tech is a leading Hyperlocal Discovery & Omni-Commerce Platform...</p>
        <p>Sekel Tech specializes in connecting businesses with local audiences...</p>
      </section>
      <section id="posts" class="posts-container">
        <h2>Recent Posts</h2>
        <div v-if="loading" class="loading-state">
          <div class="loading-spinner"></div>
          <p>Loading posts...</p>
        </div>
        <div v-else class="posts-grid">
          <div v-for="post in posts" 
               :key="post.id" 
               class="post-card">
            <h3 class="post-title">{{ post.title }}</h3>
            <div class="post-actions">
              <button @click="toggleDetails(post)" class="details-button">
                {{ post.showDetails ? 'Hide Details' : 'Show Details' }}
              </button>
            </div>
            <p v-if="post.showDetails" class="post-body">{{ post.body }}</p>
          </div>
        </div>
      </section>
    </container>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

// Google Translate initialization function
const initGoogleTranslate = () => {
  const script = document.createElement('script');
  script.type = 'text/javascript';
  script.src = 'https://translate.google.com/translate_a/element.js?cb=googleTranslateElementInit';
  script.async = true;
  document.head.appendChild(script);

  window.googleTranslateElementInit = () => {
    new window.google.translate.TranslateElement({
      pageLanguage: 'pt',
      includedLanguages: 'en,es,fr,de,it,hi,pt,jp,ar,ru,zh',
      layout: window.google.translate.TranslateElement.InlineLayout.SIMPLE,
      autoDisplay: false,
      callback: () => {
        const selectElement = document.querySelector('.goog-te-combo');
        if (selectElement) {
          selectElement.value = 'pt';
          selectElement.dispatchEvent(new Event('change'));
        }
      }
    }, 'google_translate_element');
  };
};

const posts = ref([]);
const loading = ref(true);

const toggleDetails = (post) => {
  post.showDetails = !post.showDetails;
};

const fetchPosts = async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts');
    const data = await response.json();
    posts.value = data.slice(0, 5).map(post => ({
      ...post,
      showDetails: false
    }));
    loading.value = false;
  } catch (error) {
    console.error('Error fetching posts:', error);
    loading.value = false;
  }
};

onMounted(() => {
  initGoogleTranslate();
  fetchPosts();
});
</script>

<style scoped>
#google_translate_element {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  float: right;
}

/* Posts Section */
.posts-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

/* Loading State */
.loading-state {
  text-align: center;
  padding: 20px;
}

.loading-spinner {
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
  margin: 0 auto;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* Posts Grid Layout */
.posts-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

/* Post Card Styling */
.post-card {
  background: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease;
}

.post-card:hover {
  transform: translateY(-5px);
}

.post-title {
  color: #2c3e50;
  margin: 0 0 15px 0;
  font-size: 1.2rem;
  text-transform: capitalize;
}

.post-body {
  color: #666;
  line-height: 1.6;
  margin-top: 15px;
}

.post-actions {
  margin-top: 15px;
}

.details-button {
  background-color: #3498db;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.details-button:hover {
  background-color: #2980b9;
}

/* General Styles */
#posts {
  margin-top: 20px;
}

h2, h3 {
  color: #333;
}

p {
  color: #666;
}

/* Container Styles */
container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

/* Header Styles */
header {
  text-align: center;
  padding: 40px 0;
}

header h1 {
  font-size: 2rem;
  color: #2c3e50;
  margin: 0;
}

/* Section Styles */
section {
  margin: 40px 0;
}

/* Footer Styles */
footer {
  text-align: center;
  padding: 20px 0;
  margin-top: 40px;
  border-top: 1px solid #eee;
}
</style>
