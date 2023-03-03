<template>
  <v-container class="mt-2">
    <v-row justify="center" align="center">
      <div
        style="
          width: 100%;
          display: flex;
          flex-direction: row;
          align-items: center;
          justify-content: space-between;
        "
      >
        <div class="loader">
          <p>Modern Todo List |</p>
          <div class="words">
            <span class="word"></span>
            <span class="word">Aqui você</span>
            <span class="word">organiza</span>
            <span class="word">suas</span>
            <span class="word">listas</span>
          </div>
        </div>
        <div>
          <v-icon color="white">mdi-github</v-icon>
          <v-icon color="white">mdi-linkedin</v-icon>
        </div>
      </div>
      <v-divider light></v-divider>
      <v-col cols="12">
        <alert-component
          :isAlert="isStatusAlert"
          :msg="msgAlert"
          :type="type"
        ></alert-component>
        <v-row justify="center">
          <span class="task" title="Pressione enter para adicionar!">
            <input
              type="text"
              v-model="task"
              placeholder="Inserir Tarefa"
              required
              @keyup.enter="createTask"
            />
          </span>
          <!-- <input
            class="task-date"
            type="datetime-local"
            id="agend"
            v-model="dateTask"
            @change="createTask"
            :disabled="task == ''"
          /> -->
        </v-row>
        <v-row justify="center" align="center">
          <div class="list" v-if="myList != ''">
            <ul v-for="(item, i) in myList" :key="i.id">
              <li>
                <v-row>
                  {{ item.name }} |
                  <v-chip
                    v-if="item.conclude"
                    color="green"
                    class="white--text ml-2"
                    small
                    >Concluído</v-chip
                  >
                  <v-chip v-else color="red" class="white--text ml-2" small
                    >Pendente</v-chip
                  >
                  <v-spacer></v-spacer>
                  <v-btn
                    icon
                    small
                    @click="concludeTask(item.id)"
                    :disabled="item.conclude || task.length > 0"
                    ><v-icon color="green">mdi-check</v-icon>
                  </v-btn>
                  <v-btn
                    icon
                    small
                    @click="deleteTask(item.id)"
                    :disabled="item.conclude"
                    ><v-icon color="red">mdi-delete</v-icon>
                  </v-btn>
                </v-row>
              </li>
            </ul>
          </div>
          <div class="no-task subtitle-1 font-weight-medium text-h6" v-else>
            <!-- <img src="../assets/undraw_to_do_list_re_9nt7.svg" /> -->
            <p>Não há tarefas cadastradas</p>
          </div>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import AlertComponent from "./AlertComponent.vue";
export default {
  components: { AlertComponent },
  name: "ListView",
  data: () => ({
    task: "",
    dateTask: "",
    myList: [],
    isStatusAlert: false,
    type: "",
  }),
  computed: {
    dateNow() {
      let date = new Date();
      let day = date.getDate().toString();
      let dayF = day.length == 1 ? "0" + day : day;
      let month = (date.getMonth() + 1).toString();
      let monthF = month.length == 1 ? "0" + month : month;
      let year = date.getFullYear().toString();
      return `${dayF}/${monthF}/${year}`;
    },
  },
  mounted() {},
  methods: {
    createTask(e) {
      e.preventDefault();
      if (this.task == "") {
        this.isStatusAlert = true;
        this.type = "error";
        this.msgAlert = "Preencha o campo primeiro!";
      } else {
        this.isStatusAlert = true;
        this.msgAlert = "Atividade inserida com sucesso!";
        this.type = "success";
        let id = Math.floor(Date.now() * Math.random()).toString(36);
        this.myList.unshift({
          id: id,
          name: this.task,
          conclude: false,
          created: this.dateNow,
          termTask: this.dateTask,
        });
        this.task = "";
        setTimeout(() => {
          this.isStatusAlert = false;
        }, 3500);
      }
    },
    deleteTask(id) {
      let result = this.myList.filter((e) => {
        return e.id != id;
      });
      return (this.myList = result);
    },
    concludeTask(id) {
      this.myList
        .filter((e) => {
          return e.id == id;
        })
        .map((e) => {
          return (e.conclude = true);
        });
    },
    reorganizarTasks() {
      let tasks = this.myList;
      let result = tasks.filter((item) => {
        return item.conclude == true;
      });
      return result;
    },
  },
  created() {},
  watch: {
    isStatusAlert() {
      if (this.isStatusAlert == true) {
        setTimeout(() => {
          this.isStatusAlert = false;
        }, 3500);
      }
    },
  },
};
</script>
<style scoped>
.todo {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-top: 2%;
}

.task {
  border-radius: 25px;
  padding: 5px;
  /* border: 1px solid gray; */
  width: 30%;
}

input {
  border-radius: 25px;
  width: 100%;
  padding: 5px;
  border: 1px solid gray;
  color: white;
}

.task-date {
  border-top-right-radius: 15px;
  border-bottom-right-radius: 15px;
  padding: 5px;
  border: 1px solid gray;
  /* width: 30%; */
}

.no-task {
  color: grey;
  margin-top: 1rem;
  justify-content: center;
}

.list {
  margin-top: 1%;
  align-items: center;
  justify-content: center;
  width: 30%;
}

.list ul {
  padding: 0px;
  margin: 0px;
}

.list ul li {
  text-decoration: none;
  list-style: none;
  padding: 18px 25px 15px;
  border-radius: 25px;
  margin-bottom: 5px;

  background: #ddd9d9;
  /* box-shadow: 20px 20px 60px #e4e2e2d2, -20px -20px 60px #ffffff; */
}

.checkbox {
  height: 25px;
  width: 25px;
  background: #eee;
  border-radius: 2px;
}

::-webkit-input-placeholder {
  color: #bbbbbb;
}

input[type="text"] {
  padding-left: 15px;
}
input[type="date"] {
  padding-left: 15px;
}
img {
  width: 50%;
}

.loader {
  color: rgb(124, 124, 124);
  font-family: "Poppins", sans-serif;
  font-weight: 500;
  font-size: 25px;
  -webkit-box-sizing: content-box;
  box-sizing: content-box;
  height: 40px;
  padding: 10px 10px;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  border-radius: 8px;
}

.words {
  overflow: hidden;
}

.word {
  display: block;
  height: 100%;
  padding-left: 6px;
  color: #956afa;
  animation: spin_4991 4s infinite;
}

@keyframes spin_4991 {
  10% {
    -webkit-transform: translateY(-105%);
    transform: translateY(-105%);
  }

  25% {
    -webkit-transform: translateY(-100%);
    transform: translateY(-100%);
  }

  35% {
    -webkit-transform: translateY(-205%);
    transform: translateY(-205%);
  }

  50% {
    -webkit-transform: translateY(-200%);
    transform: translateY(-200%);
  }

  60% {
    -webkit-transform: translateY(-305%);
    transform: translateY(-305%);
  }

  75% {
    -webkit-transform: translateY(-300%);
    transform: translateY(-300%);
  }

  85% {
    -webkit-transform: translateY(-405%);
    transform: translateY(-405%);
  }

  95% {
    -webkit-transform: translateY(-400%);
    transform: translateY(-400%);
  }
}
</style>
