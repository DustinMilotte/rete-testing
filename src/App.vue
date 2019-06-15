<template>
  <div id="app">
    <div id="rete"></div>
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
// import ConnectionMasteryPlugin from "rete-connection-mastery-plugin";

var audSocket = new Rete.Socket("AudVenture story");

class InputControl extends Rete.Control {
  constructor(key) {
    super(key);
    // this.render = "js";
    this.key = key;
  }

  handler(el, editor) {
    var input = document.createElement("input");
    el.appendChild(input);

    var text = this.getData(this.key) || "Some message..";

    input.value = text;
    this.putData(this.key, text);
    input.addEventListener("change", () => {
      this.putData(this.key, input.value);
    });
  }
}

class AudComponent extends Rete.Component {
  constructor() {
    super("AudNode");
    this.task = {
      outputs: { text: "output" }
    };
  }

  handler() {
      // ???
    var textField = document.createElement("input");
    this.appendChild(textField);
    input.value = "enter text here";
  }

  builder(node) {
    var inp1 = new Rete.Input("input", "Input", audSocket);
    var out1 = new Rete.Output("choice1", "Choice", audSocket);
    var out2 = new Rete.Output("choice2", "Choice", audSocket);
    // var ctrl = new InputControl("text");

    return (
      node
        //   .addControl(ctrl)
        .addInput(inp1)
        .addOutput(out1)
        .addOutput(out2)
    );
  }

  worker(node, inputs, outputs) {
    outputs["num"] = node.id;
    return { text: node.data.text };
  }
}

export default {
  name: "app",
  mounted() {
    (async () => {
      var container = document.querySelector("#rete");
      var components = [new AudComponent()];

      var editor = new Rete.NodeEditor("demo@0.1.0", container);
      editor.use(ConnectionPlugin);
      editor.use(VueRenderPlugin);
      editor.use(ContextMenuPlugin);
      editor.use(AreaPlugin);
      editor.use(CommentPlugin);
      editor.use(HistoryPlugin);
      // editor.use(ConnectionMasteryPlugin);

      var engine = new Rete.Engine("demo@0.1.0");

      components.map(c => {
        editor.register(c);
        engine.register(c);
      });

      var n1 = await components[0].createNode({ num: 2 });

      n1.position = [80, 200];

      editor.addNode(n1);

      editor.on(
        "process nodecreated noderemoved connectioncreated connectionremoved",
        async () => {
          console.log("process");
          await engine.abort();
          await engine.process(editor.toJSON());
        }
      );

      editor.view.resize();
      AreaPlugin.zoomAt(editor);
      editor.trigger("process");
      console.log(editor.toJSON());
    })();
  }
};
</script>

<style lang="scss">
html,
body {
  height: 100%;
  width: 100%;
}

#app {
  height: 100%;
}
xus #rete {
  height: 100vh;
  width: 100vw;
}
</style>
