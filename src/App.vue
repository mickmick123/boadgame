<template>
  <div id="app">
    <div class="container-fluid">
        <div class="row bg-dark">
          <div class="col-lg-12">
            <p class="text-center text-light display-4 pt-2" style="font-size:25px;">Boardgame List</p>
          </div>
        </div>

    </div>
    <div class="container">
      <div class="row mt-3">

        <div class="col-lg-12">
          <button class="btn btn-info float-right"  v-on:click="setModal(1, 0)">Add new</button>
        </div>
      </div>
    </div>
    <hr class="bg-info">
    <div class="alert alert-danger" v-if="errorMsg">{{msg}}</div>
    <div class="alert alert-success" v-if="successMsg">{{msg}}</div>
    <div class="row">
        <div class="col-lg-12">
          <table class="table table-bordered table-striped">
            <thead>
              <tr class="text-center bg-info text-light">
                <th>Name</th>
                <th>Players</th>
                <th>Required Play Time</th>
                <th>Total Copies</th>
                <th>Edit</th>
                <th>Delete</th>
              </tr>
            </thead>
            <tbody>
              <tr class="text-center" v-for="(games, index) in boardGames">
                <td>{{games.name}}</td>
                <td>{{games.players}}</td>
                <td>{{games.reqTime}}</td>
                <td>{{games.copies}}</td>
                <td><a href="#" class="text-success" @click="setModal(2, index)"><i class="fas fa-edit"></i></a></td>
                <td><a href="#" class="text-danger" @click="deleteGame(index)"><i class="fas fa-trash-alt"></i></a></td>
              </tr>
            </tbody>
          </table>
        </div>
    </div>
   <b-modal ref="modal" :title="modalTitle" hide-footer> 
     <div>
      <div class="form-group">
        <label >Name:</label>
        <input type="text" class="form-control" v-model="testData.name" >
      </div>
      <div class="form-group">
        <label >Number of Players:</label>
        <input type="text" class="form-control" v-model="testData.players">
      </div>
      <div class="form-group">
        <label >Required Playtime:</label>
        <input type="text" class="form-control" v-model="testData.reqTime">
      </div>
      <div class="form-group">
        <label >Number of Copies:</label>
        <input type="text" class="form-control" v-model="testData.copies">
      </div>
      <b-button class="mt-2 btn-success" block @click="sendAction()">Save</b-button>
    </div> 
  </b-modal>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      errorMsg: false,
      successMsg: false,
      msg: '',
      boardGames: [],
      gameIndex: 0,
      modalTitle: '',
      testData: {
        name: '',
        players: '',
        reqTime: '',
        copies: ''
      },
      actionCommand: ''
    }
  },
  mounted () {
    if (localStorage.getItem('boardgame') !== null) {
      this.boardGames = JSON.parse(localStorage.getItem('boardgame'))
    }
  },
  methods: {
    setModal (val, index) {
      if (val === 1) {
        this.modalTitle = 'Add New Boardgame'
        this.actionCommand = 1
        this.testData.name = ''
        this.testData.players = ''
        this.testData.reqTime = ''
        this.testData.copies = ''
      } else {
        this.modalTitle = 'Edit Boardgame'
        this.actionCommand = 2
        this.gameIndex = index
        this.testData.name = this.boardGames[index].name
        this.testData.players = this.boardGames[index].players
        this.testData.reqTime = this.boardGames[index].reqTime
        this.testData.copies = this.boardGames[index].copies
      }

      this.$refs['modal'].show()
    },
    sendAction () {
      if (this.actionCommand === 1) {
        this.createGame()
      } else {
        this.editGame()
      }
      this.boardGames = JSON.parse(localStorage.getItem('boardgame'))
    },
    createGame () {
      this.boardGames.push(this.testData)
      localStorage.setItem('boardgame', JSON.stringify(this.boardGames))
      this.msg = 'New game data has been added'
      this.successMsg = true
      this.errorMsg = false
      this.$refs['modal'].hide()
    },
    editGame () {
      this.$set(this.boardGames, this.gameIndex, this.testData)
      localStorage.setItem('boardgame', JSON.stringify(this.boardGames))
      this.msg = 'New game data has been edited'
      this.successMsg = true
      this.errorMsg = false
      this.$refs['modal'].hide()
    },
    deleteGame (index) {
      this.boardGames.splice(index, 1)
      this.msg = 'Game data has been deleted'
      this.successMsg = true
      this.errorMsg = false
      localStorage.setItem('boardgame', JSON.stringify(this.boardGames))
    }
  }
}
</script>
