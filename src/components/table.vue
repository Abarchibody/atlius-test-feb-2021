<template>
  <div>
    <div class="controls">
      <div class="inputs">
        <div>
          <div class="title">N</div>
          <input v-model="n" />
        </div>
        <div>
          <div class="title">T</div>
          <input v-model="t" />
        </div>
      </div>
      <br />
      <div class="actions">
        <!-- <button @click="create">Create</button> -->
        <button v-if="!isOn" @click="onOn">On</button>
        <button v-else @click="onOff">Off</button>
        <button @click="init">Reset</button>

        <button v-if="deg != -90" @click="deg = -90">-90deg</button>
        <button v-if="deg != 0" @click="deg = 0">0deg</button>
        <button v-if="deg != +90" @click="deg = +90">90deg</button>
      </div>
    </div>
    <br />
    <div class="screen">
      <div>{{ deg }}<sup>o</sup> </div>
    </div>
    <br />
    <br />

    <div class="container">
      <div class="top-left">0X0</div>
      <div class="top-right">0X{{ n }}</div>
      <div class="bottom-left">{{ n }}X0</div>
      <div class="bottom-right">{{ n }}X{{ n }}</div>
      <div
        class="table"
        :style="
          `${'--size:' + size + 'px'}; width: ${n * size}px; height: ${n * size}px; transform: rotateZ(${-deg}deg);`
        "
      >
        <div v-for="(n, i) in matrix" :key="i" class="row" :style="`width: 100%; height: calc(100% / ${n})`">
          <app-cell
            v-for="(m, j) in matrix[i]"
            :key="j"
            :value="m"
            :bulb="bulbs[m]"
            :main="diagonals.main.includes(m)"
            :second="diagonals.second.includes(m)"
            class="col"
            :style="`width: calc(100% / ${n}; height: 100%;transform: rotateZ(${deg}deg);`"
          >
            Math.abs(m - cells.length) + 1
          </app-cell>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import appCell from "./cell";
// import { shuffle } from "lodash";
export default {
  name: "table-1",
  components: {
    appCell
  },
  data: () => ({
    n: 7,
    t: 500,
    bulbs: [],
    size: 50,
    matrix: [],
    cells: [],
    deg: 0,
    isOn: false,
    diagonals: {
      main: [],
      second: []
    }
  }),
  computed: {},
  mounted() {
    this.init();
  },
  methods: {
    onOn: async function() {
      for (let i = this.cells.length + 1; i >= 1; i--) {
        if (!this.bulbs[i]) this.bulbs[i] = await this.onBulb(1);
        if (this.diagonals.main.includes(i)) this.deg += 90;
      }
      this.isOn = true;
    },
    onOff: async function() {
      for (let i = 1; i <= this.cells.length; i++) {
        if (this.bulbs[i]) this.bulbs[i] = await this.onBulb(0);
        if (this.diagonals.second.includes(i)) this.deg -= 90;
      }
      this.isOn = false;
    },
    onBulb: function(value) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve(value);
        }, this.t);
      });
    },
    init: function() {
      this.matrix = this.createMatric(this.n);
      this.diagonals = this.getDiagonals(this.matrix);
      this.cells = this.fillCells();
      this.bulbs = [];
      for (let i = 1; i <= this.cells.length + 1; i++) {
        this.bulbs.push(0);
      }
    },
    fillCells: function() {
      let cells = [];
      for (let i = 1; i <= this.n; i++) {
        for (let j = 1; j <= this.n; j++) {
          cells.push(j);
        }
      }

      return cells;
    },
    createMatric: function(n) {
      let result = new Array(n).fill().map(() => new Array(n).fill(""));
      let counter = 1;
      let startRow = 0;
      let endRow = n - 1;
      let startCol = 0;
      let endCol = n - 1;

      while (startCol <= endCol && startRow <= endRow) {
        for (let i = startCol; i <= endCol; i++) {
          //1
          result[startRow][i] = counter;
          counter++;
        }
        startRow++;

        for (let j = startRow; j <= endRow; j++) {
          //2
          result[j][endCol] = counter;
          counter++;
        }
        endCol--;

        for (let i = endCol; i >= startCol; i--) {
          // 3
          result[endRow][i] = counter;
          counter++;
        }
        endRow--;

        for (let i = endRow; i >= startRow; i--) {
          // 4
          result[i][startCol] = counter;
          counter++;
        }
        startCol++;
      }

      // console.log(result);
      return result;
    },
    getDiagonals: function(matrix) {
      let diagonals = {
        main: [],
        second: []
      };
      for (let i = 0; i < matrix.length; i++) {
        for (let j = 0; j < matrix.length; j++) {
          if (i == j) {
            diagonals.main.push(matrix[i][j]);
          }
          if (j == matrix.length - i - 1) {
            diagonals.second.push(matrix[i][j]);
          }
        }
      }
      return diagonals;
    }
  }
};
</script>

<style lang="scss">
.controls {
  .inputs {
    display: flex;
    flex-direction: column;

    > div {
      display: flex;
      .title {
        width: 20px;
        padding: 5px 10px;
        flex-shrink: 0;
      }
      input {
        padding: 5px 10px;
        flex: 1;
      }
      .action {
        width: 20px;
        padding: 5px 10px;
      }
    }
  }
  .actions {
    display: flex;

    button {
      padding: 5px 10px;
      cursor: pointer;
      width: 100px;
    }
  }
}
.screen {
  width: 100%;
  height: 50px;
  background-color: #eee;
  padding: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 40px;
  letter-spacing: 20px;
  text-transform: uppercase;
  sup {
    font-size: 20px;
  }
}

.container {
  position: relative;

  .top-left,
  .top-right,
  .bottom-left,
  .bottom-right {
    position: absolute;
  }

  .top-left,
  .top-right {
    top: -16px;
  }

  .bottom-left,
  .bottom-right {
    bottom: -16px;
  }

  .top-left,
  .bottom-left {
    left: -16px;
  }

  .top-right,
  .bottom-right {
    right: -16px;
  }

  .bottom-left,
  .bottom-right {
    bottom: -16px;
  }
  .table {
    display: flex;
    flex-wrap: wrap;
    border: solid 2px #555;
    transition: transform 0.1s;
    margin: 0 auto;

    .row {
      display: flex;
      .col {
        width: var(--size);
        height: var(--size);
        border: solid 1px #ccc;
        flex-shrink: 0;
        position: relative;
        &.starting {
          span {
            color: #555;
            font-weight: bold;
          }
        }
        display: flex;
        justify-content: center;
        align-items: center;
        span {
          position: absolute;
          top: 0;
          left: 0;
          font-size: 10px;
          color: #bbb;
        }
        .bulb {
          border: solid 1.5px slateblue;
          width: calc(var(--size) / 2);
          height: calc(var(--size) / 2);
          border-radius: calc(var(--size) / 4);
          display: flex;
          justify-content: center;
          align-items: center;
          &.on {
            background-color: red;
            color: white;
          }
        }
        .diagonal {
          position: absolute;
          left: 50%;
          transform: translateX(-50%);
          bottom: 0;
          width: 4px;
          height: 4px;
          border-radius: 2px;
          background: green;
          &.second {
            background-color: orange;
          }
        }
      }
    }
  }
}
</style>
