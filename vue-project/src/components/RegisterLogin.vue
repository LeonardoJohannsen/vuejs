<script>
export default {
  name: 'AuthScreen',

  data() {
    return {
      isLogin: true,
      loginData: {
        username: '',
        password: ''
      },
      signupData: {
        username: '',
        nickname: '',
        email: '',
        password: '',
        confirmPassword: ''
      },
      
    };
  },
  methods: {
    gotoHome() {
      this.$router.push({ name: 'Home' });
    },
    checkPasswordsMatch() {
      return this.signupData.password === this.signupData.confirmPassword;
    },
    handleLogin() {
      this.storedUser = JSON.parse(sessionStorage.getItem('user'));
      if (this.storedUser != null) {
        if (this.loginData.username == this.storedUser.username && this.loginData.password == this.storedUser.password) {
          alert("Login efetuado com sucesso!");
          this.gotoHome();
        } else {
          alert("Usuário ou senha não encontrados!");
        }
      }else{
        alert("Nenhum usuário cadastrado!");
        this. isLogin = false;
      }
      
    },
    handleSignup() {
      if (this.checkPasswordsMatch()) {
        sessionStorage.setItem('user', JSON.stringify(this.signupData));
        alert("Usuário cadastrado com sucesso!");
        this.gotoHome();
      } else {
        alert("As senhas não coincidem.");
      }
    }
  }
};
</script>

<template>
    <div class="auth-container">
      <div class="form-container" v-if="isLogin">
        <h1>Login</h1>
        <form @submit.prevent="handleLogin">
          <div>
            <label for="username">Usuário:</label>
            <input type="text" id="username" v-model="loginData.username" required>
          </div>
          <div>
            <label for="password">Senha:</label>
            <input type="password" id="password" v-model="loginData.password" required>
          </div>
          <button type="submit">Login</button>
          <p>Não tem uma conta? <a href="#" @click.prevent="isLogin = false">Cadastre-se</a></p>
          <p><a href="#" @click="gotoHome">Voltar ao site?</a></p>
        </form>
      </div>
  
      <div class="form-container" v-else>
        <h1>Cadastro</h1>
        <form @submit.prevent="handleSignup">
          <div>
            <label for="username">Usuário:</label>
            <input type="text" id="username" v-model="signupData.username" required>
          </div>
          <div>
            <label for="nickname">Apelido:</label>
            <input type="text" id="nickname" v-model="signupData.nickname" required>
          </div>
          <div>
            <label for="email">Email:</label>
            <input type="email" id="email" v-model="signupData.email" required>
          </div>
          <div>
            <label for="password">Senha:</label>
            <input type="password" id="password" v-model="signupData.password" required>
          </div>
          <div>
            <label for="confirmPassword">Confirme a Senha:</label>
            <input type="password" id="confirmPassword" v-model="signupData.confirmPassword" required>
            <a v-if="!checkPasswordsMatch()">As senhas não coincidem.</a>
          </div>
          <button type="submit">Cadastrar</button>
          <p>Já tem uma conta? <a href="#" @click.prevent="isLogin = true">Faça login</a></p>
          <p><a href="#" @click="gotoHome">Voltar ao site?</a></p>
        </form>
      </div>
    </div>
  </template>
  
  
<style scoped>
  .auth-container {
    width: 100vw;            
    height: 100vh;           
    display: flex;           
    justify-content: center; 
    align-items: center;     
  }
  
  .form-container {
    position: sticky;
    height: max-content;
    min-width: 380px;
    max-width: 600px;
    margin-left: 10px;
    margin-right: 10px;
    padding: 20px;
    padding-left: 50px;
    padding-right: 50px;
    background-color: rgb(245, 247, 250);
    border-radius: 15px;
    font-family: Arial, Helvetica, sans-serif;
    color: rgb( 30, 30, 55);
    box-shadow: 5px 5px 5px 0px black;
  }
  
  form div {
    margin-bottom: 1rem;
  }
  
  label {
    display: block;
  }
  
  input {
    width: 100%;
    padding: 0.5rem;
    box-sizing: border-box;
    background-color: rgb(225, 227, 230);
    appearance: none;
	  border: none;
	  outline: none;
    border-color: transparent; /*bug dos pontinhos com cor da borda*/
	  border-bottom: .2em solid  rgb( 30, 30, 55);
	  border-radius: .2em .2em 0 0;
	  color:  rgb( 30, 30, 55);
    margin-bottom: 15px;
  }
  
  button {
    width: 100%;
    padding: 0.7rem;
    background-color: rgb( 30, 30, 55);
    color: white;
    border: none;
    border-radius: 15px;
    cursor: pointer;
  }
  
  button:hover {
    background-color: rgb( 60, 60, 85);
  }
  
  p {
    text-align: center;
  }
  
  a {
    color:rgb( 30, 30, 55);
    cursor: pointer;
  }
</style>
  