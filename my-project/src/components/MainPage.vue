<template>
  <div>
    <header class="header">
      <div class="profile-photo">
        <img src="../assets/photo-de-profil.jpg" alt="photo de profil" @click="scrollToTop">
      </div>
      <nav class="menu">
        <ul>
          <li><a href="#presentation" @click="scrollToTop">Présentation</a></li>
          <li><a href="#creations" @click="scrollToTop">Créations</a></li>
          <li><a href="#contact" @click="scrollToTop">Formulaire de Contact</a></li>
        </ul>
      </nav>
      <div v-if="showError" class="error-container">
        <img :src="errorImage" alt="Error 404">
      </div>
    </header>

    <div class="main-container">
      <div class="container-one">
        <div class="profile">
          <h2>Hugo Blanc</h2>
          <p>développeur web junior</p>
        </div>
        <div class="about-me">
          <h3>à propos de moi</h3>
          <p>27ans, habite en France pres de la frontière suisse actuellement en formation au CEF en developements web. mes passions sont la representation historique l'astronomie et la verrerie de laboratoire.</p>
        </div>
      </div>
      <div class="container-two">
        <div class="language-section">
          <h1>Langage étudié</h1>
          <div class="logos">
            <img src="../assets/html.png" alt="symbole html">
            <img src="../assets/css.png" alt="symbole css">
            <img src="../assets/js.png" alt="symbole js">
          </div>
        </div>
        <div class="contact-form">
          <h3>Contactez-moi</h3>
          <form @submit.prevent="sendEmail">
            <label for="name">Nom/Prénom:</label>
            <input type="text" id="name" v-model="contactForm.name" required>

            <label for="subject">Objet:</label>
            <input type="text" id="subject" v-model="contactForm.subject" required>

            <label for="message">Message:</label>
            <textarea id="message" v-model="contactForm.message" required></textarea>

            <button type="submit">Envoyer</button>
          </form>
        </div>
      </div>
    </div>

    <div class="container-three">
      <h3>Projets</h3>
      <ul>
        <li v-for="(project, index) in projects" :key="index">
          <a href="#" class="project" @click.prevent="openModal(project)">{{ project.title }}</a>
        </li>
      </ul>
    </div>

    <footer>
      <div class="reseau-sociaux">
        <div class="reseau">
          <a href="https://github.com/HugoBlancAibout" target="_blank">
            <img src="../assets/code.png" alt="logo git-hub">
            <h2>git hub</h2>
          </a>
        </div>
        <div class="reseau">
          <a href="#">
            <img src="../assets/twitter.png" alt="logo X">
            <h2>twitter</h2>
          </a>
        </div>
        <div class="reseau">
          <a href="#">
            <img src="../assets/instagram.png" alt="logo instagram">
            <h2>instagram</h2>
          </a>
        </div>
      </div>
      <div class="last-update">
        Dernière mise à jour : {{ lastUpdate }}
      </div>
    </footer>

    <div v-if="showModal" class="modal-overlay" @click="closeModal">
      <div class="modal" @click.stop>
        <button class="close-button" @click="closeModal">×</button>
        <h3>{{ selectedProject.title }}</h3>
        <p>Date de création: {{ selectedProject.date }}</p>
        <p>Technologies utilisées: {{ selectedProject.technologies.join(', ') }}</p>
        <p><a :href="selectedProject.link" target="_blank">Visiter le site ou le fichier PDF</a></p>
        <p v-if="selectedProject.github"><a :href="selectedProject.github" target="_blank">Lien vers le repository GitHub</a></p>
      </div>
    </div>
  </div>
</template>

  <script>
export default {
  data() {
    return {
      pdfLink: process.env.BASE_URL + 'Cahier des charges. Devoir.pdf',
      lastUpdate: '2024-05-20',
      contactForm: {
        name: '',
        subject: '',
        message: ''
      },
      showError: false,
      errorImage: require('../assets/error-404.png'),
      projects: [
        {
          title: 'Projet 1 Cahier des charges',
          date: '2024-05-20',
          technologies: ['HTML', 'CSS', 'JavaScript'],
          link: process.env.BASE_URL + 'portefolio/my-project/public/Cahier des charges. Devoir.pdf',
          github: 'https://github.com/HugoBlancAibout/manipuler-le-DOM'
        },
        {
          title: 'Projet 2 Manipuler le DOM',
          date: '2024-04-15',
          technologies: ['HTML', 'CSS', 'JavaScript'],
          link: 'https://github.com/HugoBlancAibout/manipuler-le-DOM',
          github: 'https://github.com/HugoBlancAibout/manipuler-le-DOM'
        },
        {
          title: 'Projet 3 Dynamiser un espace commentaire',
          date: '2024-03-10',
          technologies: ['HTML', 'CSS', 'JavaScript', 'Vue.js'],
          link: 'https://github.com/HugoBlancAibout/Devoir-espace-commentaire',
          github: 'https://github.com/HugoBlancAibout/Devoir-espace-commentaire'
        }
      ],
      selectedProject: null,
      showModal: false,
    };
  },
  methods: {
    scrollToTop() {
      window.scrollTo({
        top: 0,
        behavior: 'smooth'
      });
    },
    openModal(project) {
      this.selectedProject = project;
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.selectedProject = null;
    },
    sendEmail() {
      const emailAddress = process.env.VUE_APP_CONTACT_EMAIL;
      const mailtoLink = `mailto:${emailAddress}?subject=${encodeURIComponent(this.contactForm.subject)}&body=${encodeURIComponent(`Nom/Prénom: ${this.contactForm.name}\n\nMessage: ${this.contactForm.message}`)}`;
      window.location.href = mailtoLink;
    },
    showErrorImage(event) {
      event.preventDefault();
      this.showError = true;
    }
  }
};
</script>

<style scoped>
.header {
  background-color: rgb(120, 38, 197); 
  padding: 10px; 
  color: black; 
  display: flex;
  flex-direction: column; 
  align-items: center; 
  border-radius: 15px; 
  border-radius: 25px;
  border: 5px solid black;
}

.header img {
  cursor: pointer; 
  height: 100px;
  border-radius: 50%; 
  margin-bottom: 10px; 
}

.error-container {
  text-align: center;
  margin-top: 20px;
}

.error-container img {
  max-width: 50%;
  height: auto;
}

.menu ul {
  list-style: none; 
  margin: 0; 
  padding: 0;
  display: flex;
  gap: 20px; 
}

.menu li {
  display: inline-block; 
}

.menu a {
  color: black; 
  text-decoration: none; 
}

.menu a:hover {
  text-decoration: underline; 
}

.main-container {
  display: flex;
  flex-direction: row;
  background-color: #cecece;
  border-radius: 25px;
}

.container-one {
  background-color: rgb(120, 38, 197);
  padding: 2.5%;
  width: 30%;
  border-radius: 25px;
  border: 5px solid black; 
}

.profile img {
  height: 50px;
  border-radius: 50%;
  margin-top: 10px;
}

.reseau a {
  text-decoration: none;
  color: black;
  font-size: 10px;
}

.reseau img {
  height: 2rem;
  margin-top: 10px; 
  margin-right: 20px; 
  align-items: center;
}

.container-two {
  background-color: rgb(207, 129, 238);
  margin: 15px;
  width: 70%;
  box-shadow: 0 0 0 15px rgb(112, 33, 101);
  border-radius: 25px;
  border: 5px solid black; 
}

.about-me {
  text-align: left;
  margin-left: 5px;
}

.logos img {
  height: 35px;
}

.projects-button a {
  text-decoration: none;
  color: black;
}

.contact-form {
  margin-top: 20px;
  margin-left: 20px;
  margin-right: 20px;
}

.contact-form h3 {
  text-align: center;
}

.contact-form form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.contact-form label {
  font-weight: bold;
}

.contact-form input,
.contact-form textarea {
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

.container-three{
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  background-color: rgb(120, 38, 197);
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  position: relative;
  max-width: 500px;
  width: 100%;
}

.close-button {
  position: absolute;
  top: 10px;
  right: 10px;
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
}

.container-three {
  padding: 20px;
  background-color: rgb(120, 38, 197);
  border-radius: 10px;
  margin-top: 10px;
}

.container-three h3 {
  font-size: 1.5rem;
  color: black;
  text-align: center;
  margin-bottom: 15px;
  margin-right: 25px;
}

.container-three ul {
  list-style: none;
  padding: 0;
}

.container-three li {
  margin-bottom: 10px;
}

.container-three a.project {
  display: block;
  padding: 10px;
  background-color: #fff;
  color: #007BFF;
  text-decoration: none;
  border: 1px solid black;
  border-radius: 5px;
}

.button:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); 
}

.contact-form button {
  padding: 10px;
  background-color: rgb(120, 38, 197);
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.contact-form button:hover {
  background-color: rgb(100, 30, 160);
}

.footer {
  background-color: rgb(207, 129, 238); 
  padding: 20px; 
  text-align: center;
}

.reseau-sociaux {
  display: flex;
  flex-direction: row;
  justify-content: center; 
  align-items: flex-start; 
  background-color: rgb(207, 129, 238);
  margin-top: 10px;
  border-radius: 25px;
  border: 5px solid black; 
}

.last-update {
  color: black; 
  font-size: 0.8rem;
  margin-top: 10px;
  background-color: rgb(100, 30, 160);
  border-radius: 25px;
  border: 5px solid black; 
}
</style>