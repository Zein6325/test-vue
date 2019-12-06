<template>
  <div>
    <el-dialog title="Add new task" :visible.sync="dialogVisible" width="30%" center>
      <el-form ref="form" :model="newTask">
        <el-form-item label="Title">
          <el-input v-model="newTask.title"></el-input>
        </el-form-item>
        <el-form-item label="Content">
          <el-input v-model="newTask.content"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">Cancel</el-button>
        <el-button type="primary" @click="addNewTask()">Add</el-button>
      </span>
    </el-dialog>

    <el-row :gutter="10">
      <el-col
        :xs="containerWidth(1)"
        :sm="containerWidth(2)"
        :md="containerWidth(containers.length)"
        v-for="container in containers"
        :key="container.id"
      >
        <el-card class="box-card" shadow="always">
          <div class="clearfix" slot="header">
            <span class="status">{{container.name}}</span>
            <el-button
              v-if="container.type == 1"
              style="float: right; padding: 3px 0"
              type="text"
              @click="dialogVisible = true"
            >+ Add task</el-button>
          </div>
          <div>
            <draggable
              v-model="container.tasks"
              v-bind="dragOptions"
              :empty-insert-threshold="100"
              @start="drag = true"
              @end="drag = false"
            >
              <transition-group type="transition" :name="!drag ? 'flip-list' : null">
                <todo-item v-for="task in container.tasks" :key="task.id" v-bind="task"></todo-item>
                <div slot="footer">&nbsp;</div>
              </transition-group>
            </draggable>
          </div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import initialData from "../assets/initial-data";
import draggable from "vuedraggable";
import TodoItem from "./TodoItem";

export default {
  name: "todo-container",
  components: {
    draggable,
    TodoItem
  },

  data: function() {
    return {
      containers: [],
      dialogVisible: false,
      drag: false,
      newTask: {
        title: "",
        content: ""
      }
    };
  },

  methods: {
    containerWidth(mul) {
      return 24 / (mul % 5);
    },

    addNewTask() {
      let newId =
        Math.max.apply(
          this,
          this.containers.map(x => Math.max.apply(this, x.tasks.map(y => y.id)))
        ) + 1;

      this.containers
        .find(x => x.type === 1)
        .tasks.push({
          id: newId,
          title: this.newTask.title,
          content: this.newTask.content
        });

      this.dialogVisible = false;

      this.newTask = {
        title: "",
        content: ""
      };
    }
  },
  
  computed: {
    dragOptions() {
      return {
        animation: 200,
        group: "description",
        disabled: false,
        ghostClass: "ghost"
      };
    }
  },

  beforeMount: function() {
    this.containers = [...initialData];
  }
};
</script>

<style>
.status {
  color: #0e4d3d;
  font-weight: 700;
  padding: 10px;
  font-size: 18px;
}

.title {
  color: #0c9;
  font-weight: 700;
  font-size: 16px;
}
.button {
  margin-top: 35px;
}
.flip-list-move {
  transition: transform 0.5s;
}
.no-move {
  transition: transform 0s;
}
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
.h-10 {
  min-height: 10px;
}
</style>