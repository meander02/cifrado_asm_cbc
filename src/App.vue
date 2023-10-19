<template>
  <div class="contenedor-cifrado">
    <h1>Cifrado simétrico en Vue</h1>

    <div class="contenedor-input">
      <label for="texto">Texto a cifrar:</label>
      <input type="text" v-model="data" id="texto" class="entrada-personalizada" />
    </div>

    <div class="contenedor-botones">
      <button @click="encryptData" class="boton-personalizado">Cifrar</button>
      <button @click="decryptData" class="boton-personalizado">Descifrar</button>
    </div>

    <div class="contenedor-resultado">
      <div v-if="encryptedData">
        <p style="color: rgb(210, 38, 172);">Texto cifrado:</p>
        <pre>{{ encryptedData }}</pre>
      </div>
      <div v-if="decryptedData">
        <p style="color: rgb(33, 158, 75);">Texto descifrado:</p>
        <pre>{{ decryptedData }}</pre>
      </div>
    </div>
  </div>
</template>

<script>
import CryptoJS from "crypto-js";


export default {
  data() {
    return {
      data: "",
      encryptedData: "",
      decryptedData: "",
      key: "holasoyelclave", // Reemplaza con tu clave secreta
    };
  },
  methods: {
    encryptData() {
      const cipher = CryptoJS.AES.encrypt(this.data, this.key, {
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7,
      });
      this.encryptedData = cipher.toString();
      this.decryptedData = ''; // Limpiar datos descifrados
    },
    decryptData() {
      try {
        const decipher = CryptoJS.AES.decrypt(this.encryptedData, this.key, {
          mode: CryptoJS.mode.CBC,
          padding: CryptoJS.pad.Pkcs7, // Usa Pkcs7 para el descifrado
        });
        this.decryptedData = decipher.toString(CryptoJS.enc.Utf8);
      } catch (error) {
        // Manejar cualquier error de descifrado aquí
        console.error("Error al descifrar:", error);
        this.decryptedData = "Error al descifrar";
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1 {
  font-size: 2em;
  margin-bottom: 20px;
}

.contenedor-cifrado {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
}

.contenedor-input {
  text-align: left;
  margin: 10px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 100%;
  max-width: 400px;
}

.contenedor-input label {
  display: block;
  font-size: 1.2em;
  margin-bottom: 5px;
}

.entrada-personalizada {
  width: 80%;
  padding: 10px;
  font-size: 1em;
  border: none;
  border: 1px solid #3490dc;
  border-radius: 5px;
}

.contenedor-botones {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 10px;
}

.boton-personalizado {
  padding: 10px 20px;
  font-size: 1em;
  background-color: #3490dc;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.boton-personalizado:hover {
  background-color: #2779bd;
}

.contenedor-resultado {
  text-align: left;
}
</style>
