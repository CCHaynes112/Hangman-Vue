<template>
    <div>
      <div class="Board">
        <div>
          <div class="ChosenLettersContainer">
            <p v-for="letter in attemptedLetters" :key="letter" style="font-size: 30px">
                {{ letter.toUpperCase() }}
            </p>
          </div>
          <div class="AvailableLettersContainer">
              <LetterTile v-for="letter in availableLetters" :key="letter" @clicked="onTileClicked" :letter="letter" :chosen="false"/>
          </div>
        </div>
        <HangmanPanel :word="currentVisibleWord" :failCount="failCount" />
      </div>
      <Modal v-if="showModal" :won="won" :currentWord="currentWord" />
    </div>
</template>

<script>
import HangmanPanel from './HangmanPanel';
import LetterTile from './LetterTile';
import Modal from './Modal';

export default {
    name: 'Board',
    components: {
        HangmanPanel,
        LetterTile,
        Modal
    },
    data() {
        return {
            attemptedLetters: [],
            currentWord: "",
            currentVisibleWord: "",
            won: false,
            failCount: 0,
            showModal: false,
            availableLetters: [
                "a", "b", "c", "d", "e", "f", "g","h", "i", "j", "k", "l", "m", "n", "o", "p",
                "q", "r", "s", "t", "u", "v", "w", "x", "y", "z",
            ],
            availableWords: [
                "titanic",
                "bambi",
                "jaws",
                "batman",
                "cinderella",
                "up",
                "aladdin",
                "psycho",
                "goodfellas",
                "alien",
                "halloween",
                "rocky",
                "spaceballs",
                "poltergeist",
                "shrek",
                "inception",
                "speed",
                "predator",
            ]
        }
    },
    mounted() {
        // Set up underlined representation of the current word
        let word = this.availableWords[Math.floor(Math.random() * this.availableWords.length)];
        this.currentWord = word;
        let tempWord = "";
        for (let i = 0; i < word.length; i++) {
            if (word[i] == " ") {
                tempWord += " ";
            } else {
                tempWord += "_";
            }
        }
        this.currentVisibleWord = tempWord;
    },
    methods: {
        replaceAt(str, index, replacement) {
            // Places replacement at index of str
            return str.substr(0, index) + replacement + str.substr(index + replacement.length);
        },

        onTileClicked(letter) {
            // Move letter from available letters, to used letters
            this.attemptedLetters.push(letter);
            this.availableLetters = this.availableLetters.filter((elem) => {
                return elem != letter;
            })
            if (this.currentWord.includes(letter)) {
                let tempWord = this.currentVisibleWord;
                for (let i = 0; i < this.currentWord.length; i++) {
                    if (this.currentWord[i] == letter) {
                        tempWord = this.replaceAt(tempWord, i, letter);
                    }
                }
                this.currentVisibleWord = tempWord;
                // Win condition
                if (!tempWord.includes("_")) {
                    this.won = true;
                    this.showModal = true;
                }
            }
            // Lose condition
            else {
                this.failCount++;
                if (this.failCount == 6) {
                    this.showModal = true;
                }
            }
        },
    }
}
</script>

<style scoped>
.Board {
  display: grid;
  grid-template-rows: auto auto;
}
@media only screen and (min-width: 800px) {
  .Board {
    grid-template-columns: 40vw 55vw;
  }
}
@media only screen and (max-width: 799px) {
  .Board {
    grid-template-columns: repeat(40vw 55vw);
  }
}
.AvailableLettersContainer {
    background-color: white;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}
.ChosenLettersContainer {
    background-color: white;
  display: flex;
  flex-wrap: wrap;
  align-content: flex-start;
  letter-spacing: 10px;
  padding: 10px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  height: fit-content;
  margin: 10px;
  min-height: 100px;
}
</style>
