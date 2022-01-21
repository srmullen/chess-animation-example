<script lang="ts">
  import { onMount } from 'svelte';
  import paper, { Path, Tween } from 'paper';

  export let width: number | string = 800;
  export let height: number | string = 600;

  let canvas: HTMLCanvasElement;

  let cols = 10;
  let rows = 10;
  const size = 50;

  $: position = [0, 0];

  let board: typeof Path[][];
  let piece: paper.Path;

  $: if (board) {
    const tween = piece.tweenTo({
      position: [position[0] * size + size / 2, position[1] * size + size / 2]
    }, {
      duration: 1000,
      easing: 'easeInOutCubic'
    });
    tween.start();
  }

  onMount(() => {
    paper.setup(canvas);

    board = createBorad();
  });

  function createBorad() {
    const grid = [];

    for (let i = 0; i < cols; i++) {
      const col = [];
      grid.push(col);
      const x = i * size;
      for (let j = 0; j < rows; j++) {
        const y = j * size;
        const square = new Path.Rectangle({
          fillColor: ['black', 'white'][(i + j) % 2],
          from: [x, y],
          to: [x + size, y + size]
        });
        square.onClick = function (event) {
          position = [i, j];
        }
        col.push(square);
      }
    }

    piece = new Path.Circle({
      center: [position[0] * size + size / 2, position[1] * size + size / 2],
      radius: 10,
      fillColor: 'red'
    });

    return grid;
  }
  
</script>

<canvas class="m-auto" {width} {height} bind:this={canvas}></canvas>