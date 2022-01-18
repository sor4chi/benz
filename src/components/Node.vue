<template>
  <div class="node">
    <form
      class="node-info"
      :ref="`node-${tree.id}`"
      @click.stop="clickEditHandler()"
      @submit.prevent="submitEdithandler(tree.id)"
    >
      <span
        class="remove-node"
        @click.stop="clickRemoveHandler(tree.id, parentId)"
      >
        <span></span>
        <span></span>
      </span>
      <!-- <p>
        {{ tree.id }}
      </p> -->
      <p v-if="!isEdit">
        {{ tree.name }}
      </p>
      <input class="edit-node" type="text" v-model="editName" v-if="isEdit" />
      <span class="add-node" @click.stop="clickAddHandler(tree.id, 'added')">
        <span></span>
        <span></span>
      </span>
    </form>
    <ul>
      <li v-for="child in tree.children" :key="child.id">
        <Node
          :tree="child"
          :addNode="addNode"
          :removeNode="removeNode"
          :editNode="editNode"
          :parentId="tree.id"
        />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isEdit: false,
      editName: "",
      changedTree: this.rootTree
    };
  },
  props: {
    tree: {
      type: Object,
      required: true
    },
    addNode: {
      type: Function,
      required: true
    },
    removeNode: {
      type: Function,
      required: true
    },
    editNode: {
      type: Function,
      required: true
    },
    parentId: {
      type: Number,
      required: true
    }
  },
  methods: {
    clickAddHandler(id, name) {
      this.addNode(name, id);
    },
    clickRemoveHandler(id, parent_id) {
      this.removeNode(id, parent_id);
    },
    clickEditHandler() {
      this.isEdit = true;
      this.editName = this.tree.name;
    },
    submitEdithandler(id) {
      this.isEdit = false;
      this.editNode(id, this.editName);
      this.editName = "";
    }
  }
};
</script>

<style>
.node {
  margin: 10px;
  padding: 30px;
  width: max-content;
  border: 1px solid #ccc;
  border-radius: 100%;
}
.node-info {
  border: 1px solid rgb(179, 179, 179);
  border-radius: 10px;
  width: max-content;
  min-width: 100px;
  margin: auto;
  position: relative;
}
.tree > .node > .node-info > span.remove-node {
  display: none;
}
.add-node {
  position: absolute;
  left: 0;
  right: 0;
  bottom: -10px;
  margin: auto;
  width: 20px;
  height: 20px;
  border-radius: 100%;
  background-color: rgb(179, 202, 218);
}
.add-node span:first-child {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin: auto;
  width: 13px;
  height: 3px;
  background-color: rgb(255, 255, 255);
}
.add-node span:last-child {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin: auto;
  width: 3px;
  height: 13px;
  background-color: rgb(255, 255, 255);
}
.remove-node {
  position: absolute;
  left: 0;
  right: 0;
  top: -10px;
  margin: auto;
  width: 20px;
  height: 20px;
  border-radius: 100%;
  background-color: rgb(218, 179, 206);
}
.remove-node span:first-child {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin: auto;
  width: 13px;
  height: 3px;
  background-color: rgb(255, 255, 255);
}
.edit-node {
  width: 94px;
  border: 1px solid rgb(179, 179, 179);
  border-radius: 5px;
  padding: 2px;
  height: 17px;
  margin-bottom: 15px;
  text-align: center;
}
path {
  stroke: #333;
  stroke-width: 2;
  fill: none;
}
</style>