<template>
  <link
    rel="stylesheet"
    href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.2.0/css/all.min.css"
    integrity="sha512-xh6O/CkQoPOWDdYTDqeRdPCVd1SpvCA9XXcUnZS2FmJNp1coAFzvtCN9BmamE+4aHK8yyUHUSCcJHgXloTyT2A=="
    crossorigin="anonymous"
    referrerpolicy="no-referrer"
  />
  <!-- CSS only -->
  <link
    href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/css/bootstrap.min.css"
    rel="stylesheet"
    integrity="sha384-iYQeCzEYFbKjA/T2uDLTpkwGzCiq6soy8tYaI1GyVh/UjpbCx/TYkiZhlZB6+fzT"
    crossorigin="anonymous"
  />
  <!-- <link rel="preload" href="https://unpkg.com/@luxdamore/vue-fake3d-image-effect@latest/dist/Fake3dImageEffect.css" as="style" onload="this.rel='stylesheet'" />
    <link rel="preload" href="https://unpkg.com/@luxdamore/vue-fake3d-image-effect@latest/dist/Fake3dImageEffect.umd.min.js" as="script" /> -->
   
  <div class="container todo-container">
    
    
  
      <header class="bg-secondary p-3">
        <h1 class="fs-1 text-center text-white border-bottom p-3">To-Do</h1>
      </header>

      <main>
        <transition class="animate__animated animate__swing">
          <toDo v-if="taskEmpty" />
        </transition>
        <div class="box">
          <!-- ||input filed to add the to do text + the addbtn || -->
          <form @submit.prevent="Addlist">
            <div class="input-group mb-3">
              <input
                type="text"
                class="form-control input-bar"
                v-model="text"
                placeholder="What you wana do"
                aria-label="Recipient's username"
                aria-describedby="Add"
                autofocus
              />
              <button
            
                @click="Addlist"
                class="btn btn-outline-secondary addbtn"
                type="button"
                id="Add"
                
              >
                Add
              </button>
            </div>
          </form>
        </div>
        <!-- ||hier is the list|| -->
        <div class="listBox">
          <ul>
            <li
              v-for="(task, index) in tasks"
              class="border-bottom border-end border-dark mb-3 animate__animated animate__backInDown"
            >
            
              <div class="d-flex justify-content-between">
                <div class="btn-group" role="group">
                  <input
                    type="checkbox"
                    style="margin-bottom: 10px; margin-left: 10px"
                    v-model="task.done"
                  />

                  <p
                    style="margin-left: 5px"
                    :class="{ strike: task.done }"
                    v-if="!task.edit"
                  >
                    {{ task.text }}
                  </p>
                  <input
                    v-else
                    type="text"
                    v-model="task.text"
                    class="edit-input"
                  />
                </div>

                <div
                  class="btn-group"
                  style="margin: 10px"
                  role="group"
                  aria-label="Basic outlined example"
                >
                  <button
                    type="button"
                    class="btn bg-primary text-white"
                    @click="task.edit = !task.edit"
                  >
                    <i class="fa-solid fa-pen-to-square"></i>
                  </button>
                  <button
                    type="button"
                    class="btn border-dark"
                    style="margin-left: 10px"
                    @click="removeItem(index)"
                  >
                    &#88;
                  </button>
                </div>
              </div>
            </li>
          </ul>
        </div>
      </main>
   

 
     

  <footer class="d-flex justify-content-center w-100">
    <button
      type="button"
      class="btn btn-outline-primary w-50 clearbtn"
      @click="removeAllItems"
    >
      Clear list
    </button>
    <!-- <qrcode-vue :value="JSON.stringify(tasks.map(task => task.text))" :size="size" level="H"/> -->
  </footer> 
  </div>

</template>  

<script>
import toDo from "./components/toDo.vue";
import QrcodeVue from 'qrcode.vue'

export default {
  name: "app",

 

  data() {
    return {
      tasks: [],
      text: null,
      taskEmpty: false,
      value: 'https://example.com',
      size: 100,
    };
    
  },
  template: '<qrcode-vue :value="value"></qrcode-vue>',
  components: {
    toDo,
QrcodeVue,

  },
  created() {
    this.tasks = JSON.parse(localStorage.getItem("tasks") || "[]");
    const reloaded = localStorage.getItem("reloaded");
    if (reloaded !== "true") {
      localStorage.setItem("reloaded", "true");
    }
  },
  watch: {
    tasks: {
      handler() {
        localStorage.setItem("tasks", JSON.stringify(this.tasks));
      },

      deep: true,
    },
  },
  methods: {
    Addlist() {
      if (this.text === null || this.text == "") {
        this.taskEmpty = true;
        setTimeout(() => (this.taskEmpty = false), 2000);
        return;
      } else {
        this.taskEmpty = false;
      }

      //push the text insid the tasks array

      this.tasks.push({
        text: this.text,
        done: false,
        edit: false,
      });
        console.log("tasks " , this.tasks)
      localStorage.setItem("tasks", JSON.stringify(this.tasks.text));

      this.text = null;
    },
    removeItem(index) {
      this.tasks.splice(index, 1);
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    removeAllItems() {
      localStorage.clear(this.tasks);
      location.reload();
    },
  },
};
</script>
