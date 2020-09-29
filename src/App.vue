<template>
  <div id="app">
    <h1 class="main title">Kanban Board</h1>
    <div class="drag-container">
      <ul class="drag-list">
        <li class="drag-column backlog-column">
          <span class="header">
            <h1>Backlog</h1>
          </span>
          <div id="backlog-content" class="custom-scroll">
            <ul
              class="drag-item-list"
              id="backlog-list"
              @drop="drop($event)"
              @dragover="allowDrop"
              @dragenter="dragEnter(0)"
            >
              <li
                data-type="backlog"
                class="drag-item"
                v-for="(task, index) in backlogListArray"
                :key="index"
                :draggable="true"
                @dragstart="drag(task, $event)"
              >
                {{ task }}
              </li>
            </ul>
          </div>
          <div class="add-btn-group">
            <div v-if="selectedCol !== 'backlog'" class="add-btn" @click="addBtn('backlog')">
              <span class="plus-sign">+</span>
              <span>Add Item</span>
            </div>
            <div v-if="selectedCol === 'backlog'" class="add-btn-solid" @click="saveBtn('backlog')">
              <span>Save Item</span>
            </div>
            <div class="add-container">
              <div class="add-item"></div>
            </div>
          </div>
        </li>

        2
        <li class="drag-column progress-column">
          <span class="header">
            <h1>Progress</h1>
          </span>
          <div id="progress-content" class="custom-scroll">
            <ul
              class="drag-item-list"
              id="progress-list"
              @drop="drop($event)"
              @dragover="allowDrop"
              @dragenter="dragEnter(1)"
            >
              <li
                data-type="progress"
                class="drag-item"
                v-for="(task, index) in progressListArray"
                :key="index"
                :draggable="true"
                @dragstart="drag(task, $event)"
              >
                {{ task }}
              </li>
            </ul>
          </div>
          <div class="add-btn-group">
            <div class="add-btn">
              <span class="plus-sign">+</span>
              <span>Add Item</span>
            </div>
            <div class="add-btn-solid">
              <span>Save Item</span>
            </div>
            <div class="add-container">
              <div class="add-item"></div>
            </div>
          </div>
        </li>

        3
        <li class="drag-column complete-column">
          <span class="header">
            <h1>Complete</h1>
          </span>
          <div id="complete-content" class="custom-scroll">
            <ul
              class="drag-item-list"
              id="complete-list"
              @drop="drop($event)"
              @dragover="allowDrop"
              @dragenter="dragEnter(2)"
            >
              <li
                data-type="complete"
                class="drag-item"
                v-for="(task, index) in completeListArray"
                :key="index"
                :draggable="true"
                @dragstart="drag(task, $event)"
              >
                {{ task }}
              </li>
            </ul>
          </div>
          <div class="add-btn-group">
            <div class="add-btn">
              <span class="plus-sign">+</span>
              <span>Add Item</span>
            </div>
            <div class="add-btn-solid">
              <span>Save Item</span>
            </div>
            <div class="add-container">
              <div class="add-item"></div>
            </div>
          </div>
        </li>

        4
        <li class="drag-column on-hold-column">
          <span class="header">
            <h1>On-Hold</h1>
          </span>
          <div id="on-hold-content" class="custom-scroll">
            <ul
              class="drag-item-list"
              id="on-hold-list"
              @drop="drop($event)"
              @dragover="allowDrop"
              @dragenter="dragEnter(3)"
            >
              <li
                data-type="on-hold"
                class="drag-item"
                v-for="(task, index) in onHoldListArray"
                :key="index"
                :draggable="true"
                @dragstart="drag(task, $event)"
              >
                {{ task }}
              </li>
            </ul>
          </div>
          <div class="add-btn-group">
            <div class="add-btn">
              <span class="plus-sign">+</span>
              <span>Add Item</span>
            </div>
            <div class="add-btn-solid">
              <span>Save Item</span>
            </div>
            <div class="add-container">
              <div class="add-item"></div>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  mounted() {
    this.getSavedColumn();
  },


  data() {
    return {
      backlogListArray: ["Relese the course", "Sit back and relax"],
      progressListArray: ["Work on project", "Listen to music"],
      completeListArray: ["Being cool", "Getting stuff done"],
      onHoldListArray: ["Being uncool"],
      listArrays: [],
      draggedItem: null,
      currentColumn: null,
      selectedCol:null,
    };
  },
  methods: {
 addBtn(selectedCol){
   this.selectedCol = selectedCol
    console.log("selectedCol--!", selectedCol)
  },
  saveBtn(selectedCol){
     this.selectedCol = null
    console.log("bad raha hai ---!")
  },



    drag(task, e) {
      const source = e.target.dataset.type;
      const value = e.target.textContent;
      this.draggedItem = {
        value: e.target.textContent,
        source: e.target.dataset.type,
      };
      console.log(source, value);
    },

    allowDrop(e) {
      e.preventDefault();
    },
    dragEnter(column) {
      this.currentColumn = column;
      console.log(column);
    },

    removeItem() {
      console.log("tiku", this.draggedItem);
      if (this.draggedItem.source == "backlog") {
        this.backlogListArray = this.backlogListArray.filter(
          (t) => t !== this.draggedItem.value
        );
        localStorage.setItem(
          "backlogItem",
          JSON.stringify(this.backlogListArray)
        );
      } else if (this.draggedItem.source == "progress") {
        this.progressListArray = this.progressListArray.filter(
          (t) => t !== this.draggedItem.value
        );
        localStorage.setItem(
          "progressItem",
          JSON.stringify(this.progressListArray)
        );
      } else if (this.draggedItem.source == "complete") {
        this.completeListArray = this.completeListArray.filter(
          (t) => t !== this.draggedItem.value
        );
        localStorage.setItem(
          "completeItem",
          JSON.stringify(this.completeListArray)
        );
      } else {
        this.onHoldListArray = this.onHoldListArray.filter(
          (t) => t !== this.draggedItem.value
        );
        localStorage.setItem(
          "onHoldItem",
          JSON.stringify(this.onHoldListArray)
        );
      }
    },
    drop(e) {
      if (this.currentColumn == 0) {
        this.backlogListArray.push(this.draggedItem.value);
        this.removeItem();
        localStorage.setItem(
          "backlogItem",
          JSON.stringify(this.backlogListArray)
        );
      } else if (this.currentColumn == 1) {
        this.progressListArray.push(this.draggedItem.value);
        this.removeItem();
        localStorage.setItem(
          "progressItem",
          JSON.stringify(this.progressListArray)
        );
      } else if (this.currentColumn == 2) {
        this.completeListArray.push(this.draggedItem.value);
        this.removeItem();
        localStorage.setItem(
          "completeItem",
          JSON.stringify(this.completeListArray)
        );
      } else {
        this.onHoldListArray = [
          ...this.onHoldListArray,
          this.draggedItem.value,
        ];
        this.removeItem();
        localStorage.setItem(
          "onHoldItem",
          JSON.stringify(this.onHoldListArray)
        );
      }
      console.log("droped");
      e.preventDefault();
    },
    getSavedColumn() {
      if (localStorage.getItem("backlogItem")) {
        this.backlogListArray = JSON.parse(localStorage.backlogItem);
        this.progressListArray = JSON.parse(localStorage.progressItem);
        this.completeListArray = JSON.parse(localStorage.completeItem);
        this.onHoldListArray = JSON.parse(localStorage.onHoldItem);
      }
    },
    updateSaveColumn() {
      this.listArrays = [
        this.backlogListArray,
        this.progressListArray,
        this.completeListArray,
        this.onHoldListArray,
      ];
      let arrayName = ["backlog", "progress", "complete", "onHold"];
      arrayName.forEach((arrayName, index) => {
        localStorage.setItem(
          `${arrayName}Item`,
          JSON.stringify(this.listArrays[index])
        );
      });
    },
  },
};
</script>

<style>
:root {
  --column-1: #a2622d;
  --column-2: #1b6161;
  --column-3: #248224;
  --column-4: #a22d22;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  background: url("./assets/wallhaven-5d82r1.jpg");
  height: 100vh;
  background-size: cover;
}
.drag-column {
  flex: 1;
  margin: 0 10px;
  position: relative;
  background-color: rgba(0, 0, 0, 0.4);
  border-radius: 10px;
  overflow-x: hidden;
}

h1 {
  letter-spacing: 2px;
  color: white;
}

.main-title {
  text-align: center;
  font-size: 3rem;
}

ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

.drag-container {
  margin: 20px;
}

.drag-list {
  display: flex;
  align-items: flex-start;
}

/* Columns */

.backlog-column .header,
.backlog-column .solid,
.backlog-column .solid:hover,
.backlog-column .over {
  background-color: var(--column-1);
}

.progress-column .header,
.progress-column .solid,
.progress-column .solid:hover,
.progress-column .over {
  background-color: var(--column-2);
}

.complete-column .header,
.complete-column .solid,
.complete-column .solid:hover,
.complete-column .over {
  background-color: var(--column-3);
}

.on-hold-column .header,
.on-hold-column .solid,
.on-hold-column .solid:hover,
.on-hold-column .over {
  background-color: var(--column-4);
}

/* Custom Scrollbar */
.custom-scroll {
  overflow-y: auto;
  max-height: 75vh;
}

.custom-scroll::-webkit-scrollbar-track {
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.4);
  border-radius: 10px;
  background-color: rgba(255, 255, 255, 0.3);
  margin-right: 5px;
}

.custom-scroll::-webkit-scrollbar {
  width: 10px;
}

.custom-scroll::-webkit-scrollbar-thumb {
  border-radius: 10px;
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  background-color: rgba(0, 0, 0, 0.8);
}

.header {
  display: flex;
  justify-content: center;
  border-radius: 10px;
  margin: 10px;
}

.header h1 {
  font-size: 1.25rem;
}

/* Drag and Drop */
.over {
  padding: 50px 10px;
}

.drag-item-list {
  min-height: 50px;
}

.drag-item {
  margin: 10px;
  padding: 10px;
  height: fit-content;
  background-color: rgba(0, 0, 0, 0.8);
  border-radius: 10px;
  line-height: 1.5rem;
  letter-spacing: 1px;
  cursor: pointer;
  color: white;
  text-align: left;
}

.drag-item:focus {
  outline: none;
  background-color: white;
  color: black;
}

/* Add Button Group */
.add-btn-group {
  display: flex;
  justify-content: space-between;
}

.add-btn {
  margin: 10px;
  padding: 5px 10px;
  display: flex;
  align-items: center;
  cursor: pointer;
  width: fit-content;
  border-radius: 5px;
  transition: all 0.3s ease-in;
  user-select: none;
  color: white;
}

.add-btn:hover {
  background-color: rgba(255, 255, 255, 0.9);
  color: black;
}

.add-btn:active {
  transform: scale(0.97);
}

.solid {
  display: none;
}

.solid:hover {
  transition: unset;
  filter: brightness(95%);
  color: white;
}

.plus-sign {
  font-size: 1.5rem;
  margin-right: 5px;
  position: relative;
  top: -3px;
  color: white;
}

.add-container {
  margin: 10px;
  padding: 5px 10px;
  border-radius: 10px;
  background-color: rgba(255, 255, 255, 0.9);
  min-height: 100px;
  display: none;
}

.add-item {
  width: 100%;
  min-height: 100px;
  height: auto;
  background-color: white;
  border-radius: 10px;
  margin: 5px auto;
  resize: none;
  color: black;
  padding: 10px;
}

.add-item:focus {
  outline: none;
}

/* Media Query: Laptop */
@media screen and (max-width: 1800px) {
  .main-title {
    font-size: 2rem;
  }
}

/* Media Query: Large Smartphone (Vertical) */
@media screen and (max-width: 600px) {
  body {
    overflow-y: auto;
  }

  .drag-container {
    margin: 0;
  }

  .drag-list {
    display: block;
  }

  .drag-column {
    margin: 10px;
  }
}
</style>
