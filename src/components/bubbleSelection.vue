<template>
  <div class="header">
    <div class="mode">
      <button v-if="!sorting" class="select"><p>select mode</p></button>
      <div class="list">
        <button v-if="!sorting" @click="veryslow">veryslow</button>
        <button v-if="!sorting" @click="slow">slow</button>
        <button v-if="!sorting" @click="normal">normal</button>
        <button v-if="!sorting" @click="fast">fast</button>
        <button v-if="!sorting" @click="veryfast">veryfast</button>
      </div>
    </div>
    <div class="action">
      <button class="reset" @click="reset">reset</button>
      <button class="start" v-if="!sorting" @click="startBubble">
        dubbleSort
      </button>
      <button class="start" v-if="!sorting" @click="selectionSort(lists)">
        selectionSort
      </button>
    </div>
  </div>
  <h1>mode : {{ mode }}</h1>
  <input
    type="range"
    id="points"
    min="100"
    max="300"
    step="100"
    v-model="num_of_div"
  />
  <div class="container">
    <div
      :style="{ height: `${list}px` }"
      class="main"
      v-for="list in lists"
      :key="list"
      :ref="`temp${list}`"
    ></div>
  </div>
  <!-- <button @click="debug">debug</button> -->
  <h1 v-if="sorting">this is {{ type_of_sort }}</h1>
</template>

<script>
export default {
  name: "bubble",
  data() {
    return {
      lists: [],
      num_of_div: 100,
      duration: 0,
      count: 0,
      intervalArray: [],
      sorting: false,
      mode: "normal",
      type_of_sort: "",
    };
  },
  mounted() {
    this.duration = 10;
    this.count = this.duration * this.num_of_div;
    for (let i = 0; i < this.num_of_div; i++) {
      let x = Math.floor(Math.random() * 450);
      if (!this.lists.includes(x)) {
        this.lists.push(x);
      } else i--;
    }
  },
  methods: {
    veryslow() {
      this.mode = "verySlow";
      this.duration = 30;
      this.count = this.duration * this.num_of_div;
    },
    slow() {
      this.mode = "slow";
      this.duration = 20;
      this.count = this.duration * this.num_of_div;
    },
    normal() {
      this.mode = "normal";
      this.duration = 10;
      this.count = this.duration * this.num_of_div;
    },
    fast() {
      this.mode = "fast";
      this.duration = 5;
      this.count = this.duration * this.num_of_div;
    },
    veryfast() {
      this.mode = "veryFast";
      this.duration = 1;
      this.count = this.duration * this.num_of_div;
    },
    reset() {
      document.querySelector("input").disabled = false;
      this.sorting = false;
      this.intervalArray.forEach((one) => {
        clearTimeout(one);
      });
      document.querySelectorAll(".main").forEach((one) => {
        one.classList.remove("finish");
        one.classList.remove("active");
      });
      this.lists = [];
      for (let i = 0; i < this.num_of_div; i++) {
        let x = Math.floor(Math.random() * 450);
        if (!this.lists.includes(x)) {
          this.lists.push(x);
        } else i--;
      }
      this.intervalArray = [];
    },
    debug() {
      console.log(this.num_of_div);
    },
    // start the big algo
    bubblesort(x, ll, minus) {
      this.sorting = true;
      let thiss = this;
      let duration = this.duration;
      let arr = this.lists;
      for (let i = 0; i < ll - 1; i++) {
        let id = setTimeout(() => {
          document.querySelectorAll(".active").forEach((one) => {
            one.classList.remove("active");
          });
          thiss.$refs[`temp${arr[i]}`].classList.add("active");
          thiss.$refs[`temp${arr[i + 1]}`].classList.add("active");
          if (arr[i] > arr[i + 1]) {
            this.swap(arr, i, i + 1);
          }
        }, i * duration);
        this.intervalArray.push(id);
      }
      if (!(ll === this.lists.length)) {
        thiss.$refs[`temp${arr[ll]}`].classList.add("finish");
      }
      ll--;
      // the x is to change this.count to the next intervall
      x = x - minus;
      if (x == 0) {
        document.querySelectorAll(".main").forEach((one) => {
          one.classList.add("finish");
        });
        console.log("fnishs");

        return;
      }
      let id = setTimeout(this.bubblesort.bind(null, x, ll, x / ll), x);
      this.intervalArray.push(id);
    },
    startBubble() {
      this.type_of_sort = "BubbleSort";
      document.querySelector("input").disabled = true;
      this.sorting = true;
      let id = setTimeout(
        this.bubblesort.bind(
          null,
          this.count,
          this.lists.length,
          this.count / this.num_of_div
        ),
        this.count
      );
      this.intervalArray.push(id);
    },
    // the helper function for good bubble sort
    test_the_array(arr) {
      for (let i = 0; i < arr.length; i++) {
        let j = i + 1;
        for (j; j < arr.length; j++) {
          if (arr[i] > arr[j]) return false;
        }
      }
      return true;
    },
    swap(arr, x, y) {
      let temp = arr[x];
      arr[x] = arr[y];
      arr[y] = temp;
    },
    selectionSort(arr) {
      this.type_of_sort = "selectionSort";
      this.sorting = true;
      document.querySelector("input").disabled = true;
      let x = 0;
      let dynamic = this.num_of_div * this.duration;
      for (let i = 0; i < arr.length; i++) {
        let thiss = this;
        x += this.duration;
        let id = setTimeout(function () {
          console.log("start");
          let lowest = i;
          for (let j = i + 1; j < arr.length; j++) {
            let idd = setTimeout(function () {
              document.querySelectorAll(".active").forEach((one) => {
                one.classList.remove("active");
              });

              thiss.$refs[`temp${arr[lowest]}`].classList.add("active");
              thiss.$refs[`temp${arr[j]}`].classList.add("active");
              if (arr[lowest] > arr[j]) {
                lowest = j;
              }
              if (j == arr.length - 1) {
                thiss.$refs[`temp${arr[lowest]}`].classList.add("finish");
                if (i !== lowest) {
                  thiss.swap(arr, i, lowest);
                }
              }
            }, j * thiss.duration - i * thiss.duration);
            thiss.intervalArray.push(idd);
            if (i == arr.length - 2) {
              console.log("hi");

              document.querySelectorAll(".main").forEach((one) => {
                one.classList.add("finish");
              });
            }
          }
        }, ((dynamic + dynamic - x) * i) / 2);
        this.intervalArray.push(id);
      }
      return arr;
    },
  },
  watch: {
    num_of_div: function () {
      this.num_of_div = Number(this.num_of_div);
      this.count = this.duration * this.num_of_div;
      this.reset();
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
input {
  cursor: pointer;
}
button {
  cursor: pointer;
  transition: all 0.4s;
  padding: 5px;
  text-transform: capitalize;
}
.container {
  margin-top: 10px;
  background: #eee;
  display: flex;
  justify-content: flex-end;
  transform: rotate(180deg);
  height: 460px;
}

.main {
  background: black;
  width: 12px;
  margin: 1px;
}
h1 {
  text-align: center;
}
.active {
  background: red;
}
.finish {
  background: blue;
}
/* start my style */

.header {
  width: 80%;
  margin: auto;
  height: 50px;
  display: flex;
  justify-content: space-between;
  background-color: gray;
}
.header .mode {
  width: 150px;
}
.header .mode .select {
  background: black;
  color: white;
  border: none;
  outline: none;
  width: 100%;
  height: 100%;
  font-size: 20px;
}
.header .mode .select:hover {
  background: rgb(59, 59, 59);
}
.header .mode:hover .list {
  display: block;
}
.header .mode .list {
  position: relative;
  z-index: 1;
  display: none;
}
.header .mode .list button {
  display: block;
  width: 100%;
  height: 30px;
  background: black;
  color: white;
  border: none;
  outline: none;
}
.header .mode .list button:hover {
  background: white;
  color: black;
  border: 1px solid black;
}
.action {
  display: flex;
  width: 300px;
}
.action button {
  width: 150px;
  height: 100%;
  border: none;
  outline: none;
  background: black;
  color: white;
  font-size: 20px;
}
.action button:hover {
  background: rgb(59, 59, 59);
}
.action .start {
  margin: 0 1px;
}
</style>

