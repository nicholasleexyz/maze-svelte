<script>
  const ROOM_COLUMNS = 8;
  export const TILE_COLUMNS = ROOM_COLUMNS * 2 + 1;

  const PASSAGE_TILE = ".";
  const FLOOR_TILE = " ";
  const WALL_TILE = "#";

  const tiles = Array(TILE_COLUMNS * TILE_COLUMNS).fill(WALL_TILE);
  const rooms = Array.from(
    { length: ROOM_COLUMNS * ROOM_COLUMNS },
    (_, i) => (TILE_COLUMNS + 1) * (Math.floor(i / ROOM_COLUMNS) + 1) + i * 2,
  );

  const visited = new Set();
  const stack = [0]; // room index

  const DIRECTIONS = [
    { dx: 0, dy: -1, condition: (_x, y) => y > 0 }, // Up
    { dx: 1, dy: 0, condition: (x, _y) => x < ROOM_COLUMNS - 1 }, // Right
    { dx: 0, dy: 1, condition: (_x, y) => y < ROOM_COLUMNS - 1 }, // Down
    { dx: -1, dy: 0, condition: (x, _y) => x > 0 }, // Left
  ];

  const getNeighborData = (roomIndex) => {
    const x = roomIndex % ROOM_COLUMNS;
    const y = Math.floor(roomIndex / ROOM_COLUMNS);

    return DIRECTIONS.filter(({ condition }) => condition(x, y))
      .map(({ dx, dy }) => ({
        neighbor: roomIndex + dx + dy * ROOM_COLUMNS,
        dx,
        dy,
      }))
      .filter(({ neighbor }) => !visited.has(neighbor));
  };

  while (stack.length > 0) {
    const currentRoomIndex = stack[stack.length - 1];
    visited.add(currentRoomIndex);

    const neighbors = getNeighborData(currentRoomIndex);

    if (neighbors.length > 0) {
      const { neighbor, dx, dy } =
        neighbors[Math.floor(Math.random() * neighbors.length)];
      stack.push(neighbor);

      const tileIndex = rooms[currentRoomIndex] + dx + dy * TILE_COLUMNS;
      tiles[tileIndex] = PASSAGE_TILE;
    } else {
      stack.pop();
    }

    tiles[rooms[currentRoomIndex]] = FLOOR_TILE;
  }
</script>

<main
  style="display:flex;justify-content:center;align-items:center;width:100%;height:100dvh;background-color:#181818;"
>
  <div
    style="background-color:#A0153E;width:50%;display:grid;grid-template-columns:repeat({TILE_COLUMNS},1fr);gap:0.5rem;place-items:center;"
  >
    {#each tiles as tile}
      {#if tile == "#"}
        <div
          style="width:100%;aspect-ratio:1/1;background-color:#5D0E41;outline:solid #00224d 0.5rem"
        ></div>
      {:else}
        <div
          style="width:100%;aspect-ratio:1/1;background-color:#FF204E"
        ></div>
      {/if}
    {/each}
  </div>
</main>

<style>
</style>
