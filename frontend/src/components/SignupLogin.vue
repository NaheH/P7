<template>
  <section class="card">
    <div class="card">
      <h1>Réseau social interne</h1>
      <!--Logo-->
      <img id="logo" alt="Logo de l'entreprise Groupomania" src="../assets/logo.png" />

      <h2 class="card__title" v-if="mode == 'login'">Connexion 💬</h2>
      <h2 class="card__title" v-else>Inscription</h2>

      <p class="card__subtitle" v-if="mode == 'login'">Tu n'as pas encore de compte ? <span class="card__action" @click="switchToSignup()"><strong>Créer un compte</strong></span></p>
      <p class="card__subtitle" v-else>Tu as déjà un compte ? <span class="card__action" @click="switchToLogin()"><strong>Se connecter</strong></span></p>

      <div class="form-row">
        <input v-model="email" class="form-row__input" type="text" placeholder="Adresse mail"/>
      </div>

      <div class="form-row" v-if="mode == 'create'">
        <input v-model="firstName" class="form-row__input" type="text" placeholder="Prénom"/>
        <input v-model="lastName" class="form-row__input" type="text" placeholder="Nom"/>
      </div>

      <div class="form-row">
        <input v-model="password" class="form-row__input" type="password" placeholder="Mot de passe"/>
      </div>

      <div class="form-row" v-if="mode == 'login' && status == 'error_login'">
        Adresse mail et/ou mot de passe invalide
      </div>

      <div class="form-row" v-if="mode == 'create' && status == 'error_create'">
        Adresse mail déjà utilisée
      </div>

      <div class="form-row">
          <button @click="login()" class="button" :class="{'button--disabled' : !validatedFields}" v-if="mode == 'login'">
            <span v-if="status == 'loading'">Connexion en cours...</span>
            <span v-else>Connexion</span>
          </button>
          <button @click="signup()" class="button" :class="{'button--disabled' : !validatedFields}" v-else>
            <span v-if="status == 'loading'">Création en cours...</span>
            <span v-else>Créer mon compte</span>
          </button>
      </div>
    </div>
  </section>
</template>

<script>

import { mapState } from 'vuex'


export default {
  name: 'SignupLoginView',
  data: function () {
    return {
      mode: 'login',
      email: '',
      lastName: '',
      firstName: '',
      password: '',
    }
  },
  computed: {
    validatedFields: function () {
      if (this.mode == 'create') {
        if (this.email != "" && this.lastName != "" && this.firstName != "" && this.password != "") {
          return true;
        } else {
          return false;
        }
      } else {
        if (this.email != "" && this.password != "") {
          return true;
        } else {
          return false;
        }
      }
    },
    ...mapState(['status'])
  },
  methods: {
    switchToSignup: function () {
      this.mode = 'create';
    },
    switchToLogin: function () {
      this.mode = 'login';
    },
    login: function () {
      const self = this;
      this.$store.dispatch('login', {
        email: this.email,
        password: this.password,
      }) .then(function () {
        self.$router.push('list');
      }, function (error) {
        console.log(error);
      })
    },
    signup: function () {
      this.$store.dispatch('signup', {
        email: this.email,
        lastName: this.lastName,
        firstName: this.firstName,
        password: this.password,
      }) .then(function (response) {
        console.log(response);
      }, function (error) {
        console.log(error);
      })
    }
  }
}

</script>



<style scoped>

.form-row {
  display: flex;
  margin: 16px 0px;
  gap:16px;
  flex-wrap: wrap;
}
.form-row__input {
  padding:8px;
  border: none;
  border-radius: 8px;
  background:#f2f2f2;
  font-weight: 500;
  font-size: 16px;
  flex:1;
  min-width: 100px;
  color: black;
}
.form-row__input::placeholder {
  color:#aaaaaa;
    
}

</style>
