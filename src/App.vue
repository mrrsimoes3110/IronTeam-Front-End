<template>
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FastAPI</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.1/dist/css/bootstrap.min.css" rel="stylesheet"
      integrity="sha384-4bw+/aepP/YC94hEpVNVgiZdgIC5+VKNBQNGCHeKRQN+PtmoHDEXuppvnDJzQIu9" crossorigin="anonymous">
  </head>
  <nav class="navbar bg-primary">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">
        <img src="./assets/logo.jpg" alt="Logo" width="200" class="d-inline-block">
        <span class="m-5 h3 text-white">IronTeam Sensores</span>
      </a>
    </div>
  </nav>

  <main class="container">
    <div class="row">
      <div class="col">
        <div class="card border-success text-bg-light m-3">
          <div class="card-header h2 text-center text-white bg-success">HR1</div>
          <div class="card-body">
            <p class="card-text h1 text-center" id="sensor1">{{ message1 }}</p>
          </div>
        </div>
      </div>
      <div class="col">
        <div class="card border-success text-bg-light m-3">
          <div class="card-header text-white h2 text-center bg-success">Temperatura</div>
          <div class="card-body">
            <p class="card-text h1 text-center" id="sensor2">{{ message2 }}</p>
          </div>
        </div>
      </div>
    </div>
  </main>
  <footer class="bg-dark text-white mt-5 ">
    <div class="bg-primary text-center py-3">
      <p class="mb-0">Exemplo dados coletados em Websocket enviados por MQTT</p>
    </div>
  </footer>
</template>

<script>
export default {
  data() {
    return {
      message: String,
      message1: "Aguardando valor...",
      message2: "Aguardando valor...",
      splitResult: [],
      socket: null,
    };
  },
  computed: {
    cardBodyStyle() {
      return {
        backgroundColor: this.message === "Aguardando valor..." ? "#f8f9fa" : "#e2f0cb", // Exemplo de cores diferentes
        color: this.message === "Aguardando valor..." ? "#212529" : "#155724", // Exemplo de cores diferentes
      };
    },
  },
  created() {
    this.connectWebSocket();
  },
  methods: {
    connectWebSocket() {
      this.socket = new WebSocket("ws://134.122.16.213:8001/ws");

      this.socket.onmessage = (event) => {
        const message = event.data;
        this.updateMessage(message);
      };
    },
    updateMessage(message) {  
      this.message = message;
      this.splitResult = this.message.split(" ");
      if (this.splitResult[0] == "hr1"){
        this.message1 = this.splitResult[1];
      }else
      {
        this.message2 = this.splitResult[1];
      }
    },
  },
};
</script>

<style>
/* Estilos para os cards do Bootstrap */

</style>
