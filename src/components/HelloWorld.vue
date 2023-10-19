<script setup>
defineProps({
  msg: String,
})

</script>
<template>
  <div>
    <h1>{{ msg }}</h1>
    <div>
      <label>Mensaje:</label>
      <input type="text" id="texto" v-model="mensaje" />
    </div>
    <div>
      <label>Clave:</label>
      <input type="number" id="clave" v-model="clave" />
    </div>
    <br>
    <button class="b2" @click="cifrar">Cifrar</button>

    <button class="b1" @click="descifrar">Descifrar</button>
    <div>
      <p>Mensaje Original: {{ mensaje }}</p>
      <p v-if="mensajeCifrado">Cifrado/Descifrado: {{ mensajeCifrado }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      mensaje: "",
      clave: 2,
      mensajeCifrado: ""
    };
  },
  methods: {
    cifrar() {
      this.mensajeCifrado = escitlaCifrado(this.mensaje, this.clave);
    },
    descifrar() {
      this.mensajeCifrado = escitlaDescifrado(this.mensaje, this.clave);
    }
  }
};

function escitlaCifrado(mensaje, clave) {
    let mensajeSinEspacios = mensaje.replace(/ /g, '');
    let textoCifrado = "";
    
    for (let i = 0; i < clave; i++) {
        for (let j = i; j < mensajeSinEspacios.length; j += clave) {
            textoCifrado += mensajeSinEspacios[j];
        }
    }

    return textoCifrado;
}

function escitlaDescifrado(mensaje, clave) {
    let mensajeSinEspacios = mensaje.replace(/ /g, ''); //ELIMINA ESPACIOS 
    let columnas = Math.ceil(mensajeSinEspacios.length / clave);
    let filas = clave;
    let caracteresFaltantes = (columnas * filas) - mensajeSinEspacios.length;
    let textoDescifrado = new Array(columnas);

    let col = 0;
    let fila = 0;

    for (let i = 0; i < columnas; i++) {
        textoDescifrado[i] = new Array(filas);
    }

    for (let i = 0; i < mensajeSinEspacios.length; i++) {
        textoDescifrado[col][fila] = mensajeSinEspacios[i];
        col++;
        if (col === columnas || (col === columnas - 1 && fila >= filas - caracteresFaltantes)) {
            col = 0;
            fila++;
        }
    }

    let resultado = "";
    for (let i = 0; i < filas; i++) {
        for (let j = 0; j < columnas; j++) {
            if (textoDescifrado[j][i]) {
                resultado += textoDescifrado[j][i];
            }
        }
    }

    // Restaurar espacios en blanco si el mensaje original los tenía
    let espacioIndex = 0;
    const mensajeDescifradoArray = resultado.split('');
    for (let i = 0; i < mensaje.length; i++) {
        if (mensaje.charAt(i) === ' ') {
            mensajeDescifradoArray.splice(i, 0, ' ');
            espacioIndex++;
        }
    }
    resultado = mensajeDescifradoArray.join('');

    return resultado;
}
</script>

<style>
  h1{
    color: rgb(0, 0, 0);
  }

  div{
    background-color: rgb(255, 255, 255);
    border-radius: 2rem;
  }
  label{
    color: black;
    font-size: 20px;
  }
  p{
    color: black;
    font-size: 20px;
  }
  button{

    background-color: blueviolet;
    color:black;
    margin: 2%;
    
    }

  input{
   font-size: larger;
  }

</style>