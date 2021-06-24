<template>
  <div class="app">
    <div id="app-stencil">
      <button type="primary" @click="getData"> getData </button>
      <div data-functionId="123456" @mousedown="startDrag">123456</div>
    </div>
    <div id="app-content"></div>
  </div>
</template>

<script>
import { Graph, Addon } from "@antv/x6";
const { Dnd } = Addon;

export default {
  components: {},
  data() {
    return {
      graph: null,
      dnd: null,
    };
  },
  mounted() {
    const data = {
      // 节点
      nodes: [
        {
          id: "node1", // String，可选，节点的唯一标识
          x: 140, // Number，必选，节点位置的 x 值
          y: 140, // Number，必选，节点位置的 y 值
          width: 180, // Number，可选，节点大小的 width 值
          height: 40, // Number，可选，节点大小的 height 值
          label: "hello", // String，节点标签
          ports: {
            groups: {
              in: {
                position: "top",
                label: {
                  position: "top", // 标签位置
                },
                attrs: {
                  circle: {
                    r: 6,
                    magnet: true,
                    stroke: "#31d0c6",
                    strokeWidth: 2,
                    fill: "#fff",
                    connectionCount: 1,
                  },
                },
              },
              out: {
                position: "bottom",
                label: {
                  position: "bottom", // 标签位置
                },
                attrs: {
                  circle: {
                    r: 6,
                    magnet: false,
                    stroke: "#31d0c6",
                    strokeWidth: 2,
                    fill: "#fff",
                    connectionCount: 1,
                  },
                },
              },
            },
            items: [
              {
                id: "port1",
                group: "in",
                attrs: {
                  text: {
                    // 标签选择器
                    text: "port1", // 标签文本
                  },
                },
              },
              {
                id: "port2",
                group: "in",
                attrs: {
                  text: {
                    // 标签选择器
                    text: "port2", // 标签文本
                  },
                },
              },
              {
                id: "port3",
                group: "in",
                attrs: {
                  text: {
                    // 标签选择器
                    text: "port3", // 标签文本
                  },
                },
              },
              {
                id: "port4",
                group: "out",
                attrs: {
                  text: {
                    // 标签选择器
                    text: "port4", // 标签文本
                  },
                },
              },
              // {
              //   id: "port5",
              //   group: "out",
              // },
            ],
          },
        },
        {
          id: "node2", // String，节点的唯一标识
          x: 260, // Number，必选，节点位置的 x 值
          y: 280, // Number，必选，节点位置的 y 值
          width: 180, // Number，可选，节点大小的 width 值
          height: 40, // Number，可选，节点大小的 height 值
          label: "world", // String，节点标签
          ports: {
            groups: {
              in: {
                position: "top",
                attrs: {
                  circle: {
                    r: 6,
                    magnet: true,
                    stroke: "#31d0c6",
                    strokeWidth: 2,
                    fill: "#fff",
                  },
                },
              },
              out: {
                position: "bottom",
                attrs: {
                  circle: {
                    r: 6,
                    magnet: true,
                    stroke: "#31d0c6",
                    strokeWidth: 2,
                    fill: "#fff",
                  },
                },
              },
            },
            items: [
              {
                id: "port1",
                group: "in",
              },
              {
                id: "port2",
                group: "in",
              },
              {
                id: "port3",
                group: "in",
              },
              {
                id: "port4",
                group: "out",
              },
              // {
              //   id: "port5",
              //   group: "out",
              // },
            ],
          },
        },
      ],
      // 边
      edges: [
        {
          source: { cell: "node1", port: "port4" }, // String，必须，起始节点 id
          target: { cell: "node2", port: "port1" }, // String，必须，目标节点 id
        },
      ],
    };
    const graph = new Graph({
      container: document.getElementById("app-content"),
      // width: "100%",
      // height: "100%",
      selecting: {
        enabled: true,
        showNodeSelectionBox: true,
      },
      snapline: {
        enabled: true,
        sharp: true,
      },
      grid: true,
      connecting: {
        allowBlank: false,
        allowMulti: false,
        allowLoop: false,
        allowNode: false,
        allowEdge: false,
        allowPort: true,
        snap: true,
        validateMagnet({ cell, magnet }) {
          const disableOut = magnet.getAttribute("disable-out");
          if (disableOut) return false;
          const connectionCount = magnet.getAttribute("connection-count");
          if (!connectionCount) return true;
          const max = parseInt(connectionCount, 10);
          const outgoingEdges = graph.getOutgoingEdges(cell);
          let count = 0;
          if (outgoingEdges) {
            outgoingEdges.forEach((edge) => {
              const edgeView = graph.findViewByCell(edge);
              if (edgeView.sourceMagnet === magnet) {
                count += 1;
              }
            });
          }
          return count < max;
        },
        validateConnection({ targetMagnet }) {
          const disableIn = targetMagnet.getAttribute("disable-in");
          if (disableIn) return false;
          return true;
        },
      },
    });
    graph.fromJSON(data);
    graph.centerContent();
    this.graph = graph;
    this.dnd = new Dnd({
      target: graph,
      animation: true,
      getDropNode(node) {
        // 返回一个新的节点作为实际放置到画布上的节点
        const n = node.clone();
        const ports = {
          groups: {
            in: {
              position: "top",
              label: {
                position: "top", // 标签位置
              },
              attrs: {
                circle: {
                  r: 6,
                  magnet: true,
                  stroke: "#31d0c6",
                  strokeWidth: 2,
                  fill: "#fff",
                  connectionCount: 1,
                  disableOut: true,
                },
              },
            },
            out: {
              position: "bottom",
              label: {
                position: "bottom",
              },
              attrs: {
                circle: {
                  r: 6,
                  magnet: true,
                  stroke: "#31d0c6",
                  strokeWidth: 2,
                  fill: "#fff",
                  connectionCount: 1,
                  disableIn: true,
                },
              },
            },
          },
          items: [
            {
              id: "port1",
              group: "in",
              attrs: {
                text: {
                  text: "port1",
                },
              },
            },
            {
              id: "port2",
              group: "in",
              attrs: {
                text: {
                  text: "port2",
                },
              },
            },
            {
              id: "port4",
              group: "out",
              attrs: {
                text: {
                  text: "port4",
                },
              },
            },
          ],
        };
        n.prop("ports", ports);
        return n;
      },
    });

    this.graph.on("selection:changed", (args) => {
      args.added.forEach((cell) => {
        if (cell.isEdge()) {
          cell.isEdge() && cell.attr("line/strokeDasharray", 5); //虚线蚂蚁线
        }
      });
      args.removed.forEach((cell) => {
        cell.isEdge() && cell.attr("line/strokeDasharray", 0); //正常线
      });
    });

    const $this = this;
    document.addEventListener("keyup", function (e) {
      if (e.key === "Delete") {
        const cell = $this.graph.getSelectedCells();
        $this.graph.removeCells(cell);
      }
    });
  },
  methods: {
    getData() {
      const obj = this.graph.toJSON();
      console.log(JSON.stringify(obj));
    },
    startDrag(e) {
      const target = e.currentTarget;
      const functionId = target.getAttribute("data-functionId");
      const node = this.graph.createNode({
        width: 180, // Number，可选，节点大小的 width 值
        height: 40, // Number，可选，节点大小的 height 值
        label: "hello", // String，节点标签
        functionId,
      });

      this.dnd.start(node, e);
    },
  },
};
</script>

<style>
.x6-edge-selected {
  animation: 30s linear 0s infinite normal none running ant-line;
}
@keyframes ant-line {
  to {
    stroke-dashoffset: -1000;
  }
}
.app {
  display: flex;
  height: 98vh;
}

#app-stencil {
  width: 20%;
  border: 1px solid #f0f0f0;
  position: relative;
}
#app-stencil > div {
  width: 180px;
  height: 40px;
  border: 2px solid #31d0c6;
  text-align: center;
  line-height: 40px;
  margin: 16px;
  cursor: move;
}
#app-content {
  flex: 1;
  margin-left: 8px;
  margin-right: 8px;
  box-shadow: 0 0 10px 1px #e9e9e9;
}

.x6-graph-scroller {
  border: 1px solid #f0f0f0;
  margin-left: -1px;
}
</style>
