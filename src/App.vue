<template>
  <div id="app">
    <div id="rete"></div>
    <FormNav v-on:log-json="logJson"/>
    <AudNodeForm
      v-bind:selectedNode="selectedNode"
      v-if="formIsShowing"
      v-on:close-form="closeForm"
    />
  </div>
</template>

<script>
import Rete from "rete";
import ConnectionPlugin from "rete-connection-plugin";
import VueRenderPlugin from "rete-vue-render-plugin";
import ContextMenuPlugin from "rete-context-menu-plugin";
import AreaPlugin from "rete-area-plugin";
import CommentPlugin from "rete-comment-plugin";
import HistoryPlugin from "rete-history-plugin";
import AudNodeForm from "./components/AudNodeForm";
import FormNav from "./components/FormNav";
// import ConnectionMasteryPlugin from "rete-connection-mastery-plugin";

var audSocket = new Rete.Socket("AudVenture story");

var VueAudControl = {
  props: [],
  template: "<p></p>"
};

class InputControl extends Rete.Control {
  constructor(emitter, key, readonly) {
    super(key);
    this.component = VueAudControl;
    this.props = { emitter, ikey: key, readonly };
  }
}

class AudComponent extends Rete.Component {
  constructor() {
    super("New Node");
    this.render = "vue";
    this.task = {
      outputs: { text: "output" }
    };
  }

  builder(node) {
    var inp1 = new Rete.Input("input", "Input", audSocket);
    var out1 = new Rete.Output("Yes", "Yes", audSocket);
    var out2 = new Rete.Output("No", "No", audSocket);
    var ctrl = new InputControl(this.editor, "text");

    return node
      .addInput(inp1)
      .addControl(ctrl)
      .addOutput(out1)
      .addOutput(out2);
  }

  worker(node, inputs, outputs) {
    outputs["num"] = node.id;
    return { text: node.data.text, id: node.id };
  }
}

export default {
  name: "app",
  components: {
    AudNodeForm,
    FormNav
  },
  data() {
    return {
      formIsShowing: false,
      selectedNode: {},
      editor: null
    };
  },
  methods: {
    closeForm() {
      this.formIsShowing = false;
      console.log("close form", this.selectedNode);
    },
    showForm(e) {
      this.formIsShowing = true;
      console.log(e);
    },
    addDblClickToNodes() {
      var nodes = document.getElementsByClassName("node");
      // console.log(nodes);
      for (var i = 0; i < nodes.length; i++) {
        nodes[i].addEventListener("dblclick", this.showForm);
      }
    },
    logJson() {
      console.log("log json");
      console.log(this.editor.toJSON());
    }
  },
  mounted() {
    (async () => {
      var container = document.querySelector("#rete");
      var components = [new AudComponent()];

      this.editor = new Rete.NodeEditor("demo@0.1.0", container);
      this.editor.use(ConnectionPlugin);
      this.editor.use(VueRenderPlugin);
      this.editor.use(ContextMenuPlugin);
      this.editor.use(AreaPlugin);
      this.editor.use(CommentPlugin);
      this.editor.use(HistoryPlugin);
      // editor.use(ConnectionMasteryPlugin);

      var engine = new Rete.Engine("demo@0.1.0");

      components.map(c => {
        this.editor.register(c);
        engine.register(c);
      });

      var n1 = await components[0].createNode({
        text: "this is the text body",
        nodeID: 1
      });

      n1.position = [80, 200];

      this.editor.addNode(n1);

      this.editor.on(
        "process nodecreated noderemoved connectioncreated connectionremoved",
        async () => {
          console.log("process");
          await engine.abort();
          await engine.process(this.editor.toJSON());
          this.addDblClickToNodes();
          // console.log(editor.toJSON());
        }
      );

      this.editor.on("nodeselect", node => {
        //load selected node info to form
        console.log("whole node", node);
        this.selectedNode = node;
        // console.log("selectedNodeData from nodeselect", this.selectedNodeData);
      });

      this.editor.view.resize();
      AreaPlugin.zoomAt(this.editor);
      this.editor.trigger("process");
    })();
  }
};

// function addDblClickToNodes() {
//   var nodes = document.getElementsByClassName("node");
//   console.log(nodes);
//   for (var i = 0; i < nodes.length; i++) {
//     nodes[i].addEventListener("dblclick", showForm);
//   }
// }

// function showForm(e) {
//   console.log('show form');
//   vm.formIsShowing = true;
// }
</script>

<style lang="scss">
html,
body {
  height: 100%;
  width: 100%;
}

#app {
  height: 100%;
  background: #333;
}
xus #rete {
  height: 100vh;
  width: 100vw;
}
.control,
.input-control {
  width: 100%;
  border-radius: 10px;
  background-color: white;
  padding: 10px 6px;
  margin: 10px;
  border: 1px solid #999;
  font-size: 110%;
  width: 170px;
  height: 50px;
}
.text-preview {
  margin: 0;
  outline: none;
  border: none;
}
.selected {
  background: red;
}
</style>