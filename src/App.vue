<template>
  <div id="app">
    <Header />
    <div class="content">
      <div class="boards">
        <h1 class="board-title">
          Backlog
        </h1>
        <Container 
          group-name="boards" 
          @drag-start="handleDragStart('backlog', $event)" 
          @drop="handleDrop('backlog', $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{ className: 'placeholder'}"
        >
          <Draggable  
            v-for="card in cards.backlog"
            :key="card.id" 
          >
            <Card>
              {{card.text}}
            </Card>
          </Draggable>
        </Container>
      </div>
      <div class="boards">
        <h1 class="board-title">
          Production
        </h1>
       <Container 
        group-name="boards"
        @drag-start="handleDragStart('production', $event)" 
        @drop="handleDrop('production', $event)"
        :get-child-payload="getChildPayload"
        :drop-placeholder="{ className: 'placeholder'}"
      >
          <Draggable  
            v-for="card in cards.production"
            :key="card.id" 
          >
            <Card>
              {{card.text}}
            </Card>
          </Draggable>
        </Container>
      </div>
      <div class="boards">
        <h1 class="board-title">
          Done
        </h1>
        <Container 
          group-name="boards" 
          @drag-start="handleDragStart('done', $event)" 
          @drop="handleDrop('done', $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{ className: 'placeholder'}"
        >
          <Draggable  
            v-for="card in cards.done"
            :key="card.id" 
          >
            <Card>
              {{card.text}}
            </Card>
          </Draggable>
        </Container>
      </div>
    </div>
  </div>
</template>

<script>

import { Container, Draggable } from 'vue-smooth-dnd'

import Header from './components/Header'
import Card from './components/Card'
import mockApi from './mock_api.js'


export default {
  name: 'App',
  components: {
    Header,
    Card,
    Container,
    Draggable,
  },
  data() {
    return {
      cards: {
        backlog: mockApi.backlog,
        production: mockApi.production,
        done: mockApi.done, 
      },
      draggingCard: {
        board: '',
        index: -1,
        cardData: {},
      }
    }
  },
  methods: {
    handleDragStart(board, dragResult) {
      const { payload, isSource } = dragResult

      if (isSource) {
        this.draggingCard = {
          board, 
          index: payload.index,
          cardData: {
            ...this.cards[board][payload.index],
          }
        }
      }
    },
    handleDrop(board, dropResult) {
      const { removedIndex, addedIndex } = dropResult  

      if (board === this.draggingCard.board && removedIndex === addedIndex){
        return
      }

      if (removedIndex !== null) {
        this.cards[board].splice(removedIndex, 1)
      }

      if (addedIndex !== null) {
        this.cards[board].splice(addedIndex, 0, this.draggingCard.cardData)
      }

    },
    getChildPayload(index) {
      return {
        index,
      }
    }
  }
}
</script>
<style>
.content {
  display: flex;
  justify-content: center;
  margin: 3rem 3rem;
  align-items: flex-start;
}

.boards {
  background: var(--color-gray);
  width: 30rem;
  border-radius: 0.3rem;
  box-shadow: 0.1rem 0.2rem 0.2rem 0.1rem rgba(33, 33, 33, 0.1);
  margin: 0 2rem ;
  padding: 0 1.2rem;
}

.board-title {
  padding: 1.2rem 0.8rem;
  margin-bottom: 0.8rem;
}

.placeholder {
  background: rgba(33,33,33,0.1);
  border-radius: 0.4rem;
  margin: 0.8rem;
  transform: scaleY(0.85);
  transform-origin: 0% 0%;
}
</style>