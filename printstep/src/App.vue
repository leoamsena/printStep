<template>
  <div id="app">
    <button @click="prox()">Prox</button>
    <table>
      <tr v-for="a in altura" :key="'linha' + a">
        <td
          v-for="b in largura"
          :key="'coluna' + a + '-' + b"
          :style="{
            backgroundColor:
              arr[a - 1][b - 1].paint == 0
                ? 'transparent'
                : getCor(a - 1, b - 1),
          }"
        >
          <img src="@/assets/mark.png" v-if="a - 1 == posI && b - 1 == posJ" />
          <img
            src="@/assets/sorriso.png"
            v-else-if="arr[a - 1][b - 1].op == 'init'"
          />
        </td>
      </tr>
    </table>
    <div class="foot"></div>
  </div>
</template>

<script>
require("@/assets/sorriso.png");
require("@/assets/mark.png");
export default {
  name: "App",
  data: function () {
    return {
      largura: 16,
      altura: 16,
      arr: new Array(16),
      posI: 0,
      posJ: 0,
      cor: 1,
      passoNumero: -1,
      passos: [],
      cores: [0, 1, 2, 3, 4],
    };
  },
  methods: {
    getCor: function (i, j) {
      return this.arr[i][j].paint == 1 ? "red" : "blue";
    },
    exec: function () {
      if (Array.isArray(this.passos[this.passoNumero])) {
        this.passos[this.passoNumero][0](this.passos[this.passoNumero][1]);
      } else {
        this.passos[this.passoNumero]();
      }
      //this.passoNumero++;
      this.$forceUpdate();
    },
    prox: function () {
      this.passoNumero++;
      this.exec();
    },
    irDireita: function (num = 1) {
      this.posJ += num;
    },
    irEsquerda: function (num = 1) {
      this.posJ -= num;
    },
    irCima: function (num = 1) {
      this.posI -= num;
    },
    irBaixo: function (num = 1) {
      this.posI += num;
    },
    pintar: function () {
      console.log(this.posI, this.posJ, this.cor);
      this.$set(this.arr[this.posI][this.posJ], "paint", this.cor);
      //this.arr[this.posI][this.posJ].paint = this.cor;
    },
    lapisDireita: function (num = 1) {
      for (let i = 0; i < num; i++) {
        this.irDireita();
        this.pintar();
      }
    },
    lapisEsquerda: function (num = 1) {
      for (let i = 0; i < num; i++) {
        this.irEsquerda();
        this.pintar();
      }
    },
    mudarCor: function () {
      this.cor++;
    },
  },
  created: function () {
    this.arr = new Array(this.altura);
    for (let i = 0; i < this.altura; i++) {
      this.arr[i] = new Array(this.largura);
      for (let j = 0; j < this.largura; j++) {
        this.arr[i][j] = { op: "", paint: 0 };
      }
    }
    this.arr[0][0].op = "init";

    this.passos = [
      [this.irDireita, 6],
      this.irBaixo,
      this.pintar,
      [this.lapisDireita, 3],
      this.irBaixo,
      [this.lapisDireita, 2],
      this.irBaixo,
      this.pintar,
      this.lapisDireita,
      // linha 2
      this.irBaixo,
      this.lapisDireita,
      this.irBaixo,
      this.lapisDireita,
      this.irBaixo,
      this.pintar,
      this.irBaixo,
      this.pintar,
      this.irEsquerda,
      this.irBaixo,
      this.pintar,
      this.irEsquerda,
      //linha 3
      this.irBaixo,
      this.pintar,
      this.lapisEsquerda,
      this.irBaixo,
      this.pintar,
      this.lapisEsquerda,
      this.irEsquerda,
      this.irBaixo,
      this.pintar,
      [this.lapisEsquerda, 3],
      this.irEsquerda,
      //linha 4
      this.irCima,
      this.pintar,
      this.lapisEsquerda,
      this.irCima,
      this.pintar,
      this.lapisEsquerda,
      this.irEsquerda,
      this.irCima,
      this.pintar,
      this.irEsquerda,
      this.irCima,
      this.pintar,
      //linha 5
      this.irCima,
      this.pintar,
      this.irCima,
      this.pintar,
      this.irDireita,
      this.irCima,
      this.pintar,
      this.irCima,
      [this.lapisDireita, 2],
      this.irCima,
      this.pintar,
      //linha 6
      this.lapisDireita,
      this.mudarCor,
      [this.irBaixo, 2],
      this.lapisDireita,
      this.irBaixo,
      this.pintar,
      [this.irDireita, 2],
      this.lapisDireita,
      this.irCima,
      this.pintar,
      //linha 7
      [this.irDireita, 3],
      [this.irBaixo, 3],
      this.pintar,
      this.irBaixo,
      [this.lapisEsquerda, 3],
      this.irEsquerda,
      this.irCima,
      this.pintar,
    ];
  },
};
</script>

<style>
.foot {
  position: fixed;
  bottom: 0;
  left: 0;
}
body {
  margin: 0 50px;
  padding: 0;
}
td {
  border: 1px solid;
  margin: 0;
  width: calc(100vw / 16);
  height: calc(100vh / 16);
  text-align: center;
}
table {
  border-spacing: 0;
}
img {
  width: 50%;
  height: 100%;
}
</style>
