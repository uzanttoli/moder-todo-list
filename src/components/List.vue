<template>
  <v-container>
    <v-row justify="center" align="center">
      <p class="subtitle-1 text-h2">Modern Todo List</p>
      <v-col cols="12">
        <v-row justify="center">
          <input
            class="task"
            type="text"
            v-model="task"
            placeholder="Inserir Tarefa"
            required
          />
          <input
            class="task-date"
            type="datetime-local"
            id="agend"
            v-model="dateTask"
            @change="createTask"
            :disabled="task == ''"
          />
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
export default {
  name: "ListView",
  data: () => ({
    task: "",
    dateTask: "",
    myList: [],
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
      let id = Math.floor(Date.now() * Math.random()).toString(36);
      this.myList.unshift({
        id: id,
        name: this.task,
        conclude: false,
        created: this.dateNow,
        termTask: this.dateTask,
      });
      this.task = "";
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
  border-top-left-radius: 15px;
  border-bottom-left-radius: 15px;
  padding: 5px;
  border: 1px solid gray;
  /* width: 30%; */
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

  background: #e0e0e0;
  box-shadow: 20px 20px 60px #bebebe, -20px -20px 60px #ffffff;
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
</style>
