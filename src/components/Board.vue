<template>
  <div class="board">
    <h1 style="padding: 20px 0;">{{ msg }}</h1>
    <div class="board_inner">
      <div class="column" v-for="(column, i) in cards">
        <div class="column_inner" >
          <div class="name">{{ names[i] }}</div>
          <div class="card" v-for="(card, j) in cards[i]" >
            <div>
              <span v-if="i !== 0"><a href="#" @click="(e) => { moveLeft(e, i, j) }" ><</a></span>
              {{ cards[i][j] }}
              <span v-if="i !== ( cards.length - 1 )"><a href="#" @click="(e) => { moveRight(e, i, j) }" >{{ `>` }}</a></span>
            </div>
          </div>
          <div><a href="#" @click="(e) => add_card_dialog(e, i)">Add Card</a></div>
        </div>
      </div>
    </div>

    <md-dialog
      :md-active.sync="showDialog"
    >
      <md-dialog-title>Add New Card to {{ names[showDialogColumn] }}</md-dialog-title>
      <md-dialog-content>
        <input class="input" v-model="new_card" placeholder="CARD TEXTを入力してください。">
      </md-dialog-content>
      <md-dialog-actions>
        <md-button class="md-primary" @click="handleCloseDialog">Close</md-button>
        <md-button class="md-primary" @click="handleSaveDialog">Save</md-button>
      </md-dialog-actions>
    </md-dialog>

  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Vue Card Board',
      names: [
        'Name A',
        'Name B',
        'Name C',
        'Name D',
      ],
      cards: [
        ['default card a1', 'default card a2'],
        ['default card b2'],
        ['default card c3'],
        ['default card d4']
      ],
      showDialog: false,
      showDialogColumn: null,
      new_card: ''
    }
  },
  methods: {
    add_card: function(e, index) {
      e.preventDefault()
      var new_card = window.prompt('Add new card')
      if( new_card && new_card.replace(' ', '') !== '' ){
        this.$data.cards[index].push( new_card )
      }
    },
    add_card_dialog: function(e, index){
      this.$data.showDialog = true
      this.$data.showDialogColumn = index
    },
    moveLeft: function(e, column_index, card_index){
      e.preventDefault()
      var move_card_value = this.$data.cards[column_index][card_index]
      this.$data.cards[column_index].splice(card_index, 1)
      this.$data.cards[column_index - 1].push( move_card_value )
    },
    moveRight: function(e, column_index, card_index){
      e.preventDefault()
      var move_card_value = this.$data.cards[column_index][card_index]
      this.$data.cards[column_index].splice(card_index, 1)
      this.$data.cards[column_index + 1].push( move_card_value )
    },
    handleCloseDialog: function(e){
      if(e){
        e.preventDefault()
      }
      this.$data.showDialog = false;
      this.$data.showDialogColumn = null;
      this.$data.new_card = '';
    },
    handleSaveDialog: function(e){
      var new_card = this.$data.new_card
      if( new_card && new_card.replace(' ', '') !== '' ){
        this.$data.cards[ this.$data.showDialogColumn ].push( new_card )
      }
      this.handleCloseDialog(e)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
  margin: 0;
  padding: 0;
}
div.board{
  background: #DDDDDD;
  width: 100%;
  height: 100%;

}
div.board_inner{
  padding-left: 25px;
}
div.column{
  display: inline-block;
  width: 25%;
  vertical-align: top;
}
div.column_inner{
  margin-right: 25px;
}
div.name{
}
div.card{
  background: #FFFFFF;
  width: 100%;
  margin: 5px 0;
}

</style>
