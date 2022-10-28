<!-- Please remove this file from your project -->
<template>
  <div class="relative flex items-top justify-center min-h-screen bg-gray-100 sm:items-center sm:pt-0">
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">
    <div class="board">
      <div
        class="board__col"
        @drop="onDrop($event, index)"
        @dragover.prevent
        @dragenter.prevent
        v-for="(_, index) in columnCards"
      >
        <div
          class="board__card"
          draggable="true"
          @dragstart="startDrag($event, card, index)"
          v-for="card in columnCards[index]"
        >
          Карточка номер {{ card.id }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NuxtTutorial',
  data() {
    return {
      columnCards: [
        [
          {
            id: 1,
          },
          {
            id: 2,
          },
          {
            id: 3,
          },
          {
            id: 4,
          }
        ],
        [
          {
            id: 5,
          },
        ],
        [
          {
            id: 6,
          },
          {
            id: 7,
          },
          {
            id: 8,
          },
        ],
      ]
    }
  },
  methods: {
    startDrag(evt, item, index) {
      evt.dataTransfer.dropEffect = 'move'
      evt.dataTransfer.effectAllowed = 'move'
      evt.dataTransfer.setData('itemID', item.id)
      evt.dataTransfer.setData('columnIndex', index)
    },
    onDrop(evt, index) {
      const itemID = evt.dataTransfer.getData('itemID')
      const columnIndex = evt.dataTransfer.getData('columnIndex')
      const itemIndex = this.columnCards[columnIndex].findIndex((item) => item.id == itemID)
      const item = this.columnCards[columnIndex][itemIndex];

      this.columnCards[index].push(item);

      if (itemIndex > -1) {
        this.columnCards[columnIndex].splice(itemIndex, 1);
      }
    },
  }
}
</script>

<style>
.board {
  width: 1680px;
  padding: 0 450px;
  display: flex;
  justify-content: space-between;
}

.board__col {
  width: 200px;
  height: fit-content;
  border: 2px solid black;
  padding-right: 2px;
}

.board__card {
  width: 100%;
  height: 100px;
  background-color: white;
  margin-bottom: 10px;
  cursor: pointer;
}
</style>
