<template>
  <div>
    <input type="button" value="reset Tree" @click="resetTree()" />
    <div class="tree">
      <Node
        :tree="tree"
        :addNode="addNode"
        :removeNode="removeNode"
        :editNode="editNode"
        :parentId="0"
      />
    </div>
    <!-- {{ tree }} -->
  </div>
</template>

<script>
import Node from "./components/Node.vue";
export default {
  name: "App",
  components: {
    Node
  },
  data() {
    return {
      newTree: {
        name: "",
        parent_id: ""
      },
      tree: {
        size: 1,
        id: 1,
        name: "root",
        children: []
      }
    };
  },
  methods: {
    addNode(n, pid) {
      const name = n;
      const parent_id = pid;
      const newTree = {
        id: ++this.tree.size,
        name: name,
        children: []
      };
      this.getNodeById(parent_id, this.tree).children.push(newTree);
    },
    removeNode(id, parent_id) {
      console.log(id, parent_id);
      const target = this.getNodeById(parent_id, this.tree).children;
      const index = target.findIndex((node) => node.id === id);
      target.splice(index, 1);
    },
    editNode(id, newName) {
      this.getNodeById(id, this.tree).name = newName;
    },
    resetTree() {
      this.tree = {
        size: 1,
        id: 1,
        name: "root",
        children: []
      };
    },
    getNodeById(id, node) {
      if (node.id === id) {
        return node;
      }
      for (let i = 0; i < node.children.length; i++) {
        const child = node.children[i];
        const result = this.getNodeById(id, child);
        if (result) {
          return result;
        }
      }
      return null;
    },
    set() {
      localStorage.setItem("tree", JSON.stringify(this.tree));
    }
  },
  watch: {
    tree: {
      handler() {
        this.set();
      },
      deep: true
    }
  },
  mounted() {
    const tree = localStorage.getItem("tree");
    if (tree) {
      this.tree = JSON.parse(tree);
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
ul {
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  list-style: none;
}
.tree {
  margin: auto;
  width: max-content;
}
</style>
