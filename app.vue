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

// Add these new state variables
const posts = ref([]);
const loading = ref(false);

// Add the fetchPosts function
const fetchPosts = async () => {
  loading.value = true;
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts');
    const data = await response.json();
    // Add showDetails property to each post
    posts.value = data.slice(0, 10).map(post => ({
      ...post,
      showDetails: false
    }));
  } catch (error) {
    console.error('Error fetching posts:', error);
  } finally {
    loading.value = false;
  }
};

// Add the toggleDetails function
const toggleDetails = (post) => {
  post.showDetails = !post.showDetails;
};

const initGoogleTranslate = () => {
  // Clear any existing cookies first
  document.cookie = 'googtrans=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;';
  document.cookie = 'googtrans=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/; domain=.' + location.hostname;
  
  // Set new cookie for Hindi
  document.cookie = 'googtrans=/auto/te; path=/';
  document.cookie = 'googtrans=/auto/te; path=/; domain=.' + location.hostname;

  // Create and append Google Translate script
  const script = document.createElement('script');
  script.type = 'text/javascript';
  script.src = 'https://translate.google.com/translate_a/element.js?cb=googleTranslateElementInit';
  script.async = true;
  document.head.appendChild(script);

  window.googleTranslateElementInit = () => {
    const translateElement = new window.google.translate.TranslateElement({
      pageLanguage: 'auto',
      includedLanguages: 'mr,hi,en,es,fr,de,it,pt,jp,ar,ru,zh,te',
      layout: window.google.translate.TranslateElement.InlineLayout.SIMPLE,
      autoDisplay: false,
    }, 'google_translate_element');

    // Force Hindi translation after widget loads
    const forceHindiTranslation = () => {
      const intervalId = setInterval(() => {
        const selectElement = document.querySelector('.goog-te-combo');
        if (selectElement) {
          selectElement.value = 'hi';
          selectElement.dispatchEvent(new Event('change'));
          
          // Clean up Google Translate elements
          const topBar = document.querySelector('.skiptranslate');
          if (topBar) {
            topBar.style.display = 'none';
          }
          document.body.style.top = '0px';
          
          clearInterval(intervalId);
        }
      }, 100);

      // Clear interval after 5 seconds if it hasn't succeeded
      setTimeout(() => clearInterval(intervalId), 5000);
    };

    // Initial attempt
    forceHindiTranslation();
  };
};

// Helper function to check if translation is working
const isTranslated = () => {
  return document.cookie.includes('googtrans=/auto/hi');
};

onMounted(() => {
  // Initialize Google Translate
  initGoogleTranslate();
  
  // Add a backup check to ensure translation is applied
  setTimeout(() => {
    if (!isTranslated()) {
      console.log('Retrying translation...');
      initGoogleTranslate();
    }
  }, 2000);

  // Rest of your mounted logic
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
