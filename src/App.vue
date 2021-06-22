<template>
  <div>
    <div id="app"></div>
  </div>
</template>

<script>
import { Graph } from "@antv/x6";

export default {
  name: "App",
  components: {},
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
      container: document.getElementById("app"),
      width: 800,
      height: 600,
      grid: true,
      connecting: {
        validateMagnet({ cell, magnet }) {
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
      },
    });
    graph.fromJSON(data);
  },
};
</script>

<style>
</style>
