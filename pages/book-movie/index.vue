<template>
  <div>
    <v-form v-model="valid" ref="form">
      <v-layout>
        <v-flex xs4 sm12 class="select mx-1">
          <span>Movie: </span>
          <select v-model="selectedMovie" required="">
            <option value="">Select a Movie</option>
            <option v-for="(movie_obj, movie) in movieNames" :value="movie">{{movie}}</option>
          </select>
        </v-flex>
        
        <v-flex xs4 sm12 class="select mx-1" :class="timings.length == 0 ? 'disabled-css' : '' ">
          <span>Timings: </span>
          <select :disabled="timings.length == 0" v-model="selectedTimings" required="">
            <option value="">Select a Timings</option>
            <option v-for="(time_obj, time) in timings">{{time}}</option>
          </select>
        </v-flex>
        <v-flex xs4 sm12 class=" select mx-1" :class="seats.length == 0 ? 'disabled-css' : '' ">
          <span>Seats: </span>
          <select :disabled="seats.length == 0" v-model="selectedSeats" required="">
            <option value="">Select a Seats</option>
            <option v-for="seat in seats">{{seat}}</option>
          </select>
        </v-flex>
      </v-layout>
      <div class="text-xs-center my-3">
        <v-btn color="blue" class="white--text " @click="create.call(this, selectedMovie , selectedTimings , selectedSeats)">Book Ticket</v-btn>
      </div>
    </v-form>

    <v-data-table
      :items="tasks"
      class="elevation-1"
      hide-actions
      :headers="headers"
      v-if = "tasks.length > 0"
    >
      <template v-slot:items="props">
        <td class="text-xs-center">{{ props.item.movieName }}</td>
        <td class="text-xs-center">{{ props.item.timing }}</td>
        <td class="text-xs-center">{{ props.item.seat }}</td>
      </template>
    </v-data-table>
  </div>
</template>

<script type="text/javascript">
  export default {
    data () {
      return {
        headers: [
          { text: 'Movie Name', value: 'movieNames', align: 'center', sortable: false },
          { text: 'Timings', value: 'timings', align: 'center', sortable: false },
          { text: 'Seats', value: 'seats', align: 'center', sortable: false }
        ],
        movieNames: {
          'Shawshank Redemption': {
            '10:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10'],
            '11:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10'],
            '13:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10'],
            '17:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10'],
            '20:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10']
          },
          'Batman vs Superman': {
            '10:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10'],
            '11:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10'],
            '13:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10']
          },
          'Avengers Endgame': {
            '10:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10'],
            '11:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10'],
            '13:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10'],
            '15:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10']
          },
          'Harry Potter': {
            '07:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10'],
            '12:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10'],
            '14:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10'],
            '18:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10']
          },
          'Aquaman:': {
            '12:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10'],
            '14:00': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10']
          }
        },
        timings: [],
        seats: [],
        selectedMovie: '',
        selectedTimings: '',
        selectedSeats: '',
        tasks: [],
        valid: false
      }
    },
    watch: {
      selectedMovie: function () {
        // Clear previously selected values
        this.timings = []
        this.seats = []
        this.selectedTimings = ''
        this.selectedSeats = ''
        // Populate list of timings in the second dropdown
        if (this.selectedMovie.length > 0) {
          this.timings = this.movieNames[this.selectedMovie]
        }
      },
      selectedTimings: function () {
        // Clear previously selected values
        this.seats = []
        this.selectedSeats = ''
        // Now we have a continent and timing. Populate list of seats in the third dropdown
        if (this.selectedTimings.length > 0) {
          this.seats = this.movieNames[this.selectedMovie][this.selectedTimings]
        }
      }
    },
    methods: {
      create (name, reputation, userId) {
        if (this.movieNames.length === 0) {
          alert('Select Movie Name')
        } else if (this.timings.length === 0) {
          alert('Select Timing')
        } else if (this.seats.length === 0) {
          alert('Select Seats')
        } else {
          this.tasks.push({
            movieName: this.selectedMovie,
            timing: this.selectedTimings,
            seat: this.selectedSeats
          })

          this.selectedMovie = ''
          this.selectedTimings = ''
          this.selectedSeats = ''
        }
      }
    }
  }
</script>

<style scoped>

  /* Reset Select */
select {
  -webkit-appearance: none;
  -moz-appearance: none;
  -ms-appearance: none;
  appearance: none;
  outline: 0;
  box-shadow: none;
  border: 0 !important;
  background: #fff;
  background-image: none;
  border: 1px solid #ececec;
  padding-left: 10px;
}
/* Remove IE arrow */
select::-ms-expand {
  display: none;
}
/* Custom Select */
.select {
  position: relative;
  display: flex;
  width: 20em;
  height: 3em;
  line-height: 3;
  background: #fff;
  overflow: hidden;
  border-radius: .25em;
  border: 1px solid #ececec;
  padding-left: 10px;
}
select {
  flex: 1;
  padding: 0 .5em;
  cursor: pointer;
}
/* Arrow */
.select::after {
  content: '\25BC';
  position: absolute;
  top: 0;
  right: 0;
  padding: 0 1em;
  border: 1px solid #ececec;
  cursor: pointer;
  pointer-events: none;
  -webkit-transition: .25s all ease;
  -o-transition: .25s all ease;
  transition: .25s all ease;
}
/* Transition */
.select:hover::after {
  color: #f39c12;
}
.disabled-css {
  pointer-events: none;
  opacity: 0.7;
}
</style>