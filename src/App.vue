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
          x: 40, // Number，必选，节点位置的 x 值
          y: 40, // Number，必选，节点位置的 y 值
          width: 80, // Number，可选，节点大小的 width 值
          height: 40, // Number，可选，节点大小的 height 值
          label: "hello", // String，节点标签
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
        {
          id: "node2", // String，节点的唯一标识
          x: 160, // Number，必选，节点位置的 x 值
          y: 180, // Number，必选，节点位置的 y 值
          width: 80, // Number，可选，节点大小的 width 值
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
    });
    graph.fromJSON(data);
  },
};
</script>

<style>
</style>
