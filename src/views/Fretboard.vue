<script setup>
import { ref, computed } from 'vue'
const strings = ref(6)
const positions = ref(12)

const modeOptions = [7, 12, 18, 24]
const inColor = ref(false)
const setColors = () => { inColor.value = !inColor.value }
const colorButtonText = computed(() => { return inColor.value ? 'Colo' : 'Mono' })

const cPosition = { 1: 8, 2: 1, 3: 5, 4: 10, 5: 3, 6: 8 }
const notes = ["C", "C#", "D", "D#", "E", "F", 'F#', "G", "G#", "A", "A#", "B"]
const getNote = (string, position) => {
  const actString = string + 1
  const noteC = position > 12 ? cPosition[actString] + 12 : cPosition[actString]
  const noteIndex = position >= noteC ? position - noteC : notes.length - noteC + position
  return notes[noteIndex]
}

const playNote = (note) => {
  if (note) {
    let audio = new Audio(note)
    audio.play()
  }
}
</script>

<template>
  <main>
    <h3>Fretboard</h3>
    <div class="conainer">
      <div class="row">
        <div class="col">
          <div class="d-flex gap-3 mb-3">
            <select class="form-select" v-model="positions" aria-label="Default select example">
              <option v-for="mode in modeOptions" :value="mode" :key="mode">{{ mode }}</option>
            </select>
            <button class="btn btn-info" @click="setColors">{{ colorButtonText }}</button>
          </div>
        </div>
      </div>
    </div>
    <div class="row" v-for="(string, stringIndex) in strings" id="fretboard" :class="{ inColor }" :key="stringIndex">
      <div class="col pointer" @click="playNote('./src/assets/notes/note' + getNote(stringIndex, position - 1) + '.wav')"
        :class="[getNote(stringIndex, position - 1).toLowerCase().replace('#', 'sharp'), { 'hoo': inColor }]"
        v-for="position in positions" :key="position">
        <div class="note">{{ getNote(stringIndex, position - 1) }}</div>
      </div>
    </div>
    <div class="row mt-5">
      <div class="col" v-for="(position, positionIndex) in positions" :key="positionIndex">
        {{ positionIndex }}</div>
    </div>


  </main>
</template>

<style lang="scss" scoped>
#fretboard {
  border-top: 0.5px solid gray;

  .col {
    min-height: 50px;
    background-color: lightyellow;
    padding-left: 0;
    padding-right: 0;
    text-align: center;
    border-left: 3px solid black;
    display: flex;
    justify-content: center;
    align-items: center;
    border-bottom: 0.5px solid gray;

    .note {
      color: black;
      font-size: 14px;
      font-weight: 700;
      background-color: white;
      width: 30px;
      height: 30px;
      line-height: 30px;
      border-radius: 50%;
      border: 0.5px solid gray;
    }

    .string {
      width: 100%;
      height: 2px;
      background-color: navy;
    }
  }

  &.inColor {
    .c {
      background-color: dodgerblue;
    }

    .csharp {
      background-color: color-mix(in lch, dodgerblue 50%, lightseagreen 50%);
    }

    .d {
      background-color: lightseagreen;
    }

    .dsharp {
      background-color: color-mix(in lch, lightseagreen 50%, gold 50%);
    }

    .e {
      background-color: gold;
    }

    .f {
      background-color: beige;
    }

    .fsharp {
      background-color: color-mix(in lch, beige 50%, darkorange 50%);
    }

    .g {
      background-color: darkorange;
    }

    .gsharp {
      background-color: color-mix(in lch, darkorange 50%, crimson 50%);
    }

    .a {
      background-color: crimson;
    }

    .asharp {
      background-color: color-mix(in lch, crimson 50%, darkorchid 50%);
    }

    .b {
      background-color: darkorchid;
    }
  }

  .pointer {
    cursor: pointer;
  }
}
</style>
