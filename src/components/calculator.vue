<template lang="pug">

  div.calculator(:style="currentTheme")
    transition(name="fadein")
      div.cover(@click="themeActive = !themeActive" v-if="themeActive")

    transition(name="slidein" v-on:enter="enter" v-on:leave="leave")
      div.calculator-overlay(v-if="themeActive")
        div.calculator-theme
          div.calculator-theme__title Choose A Theme
          section.calculator-theme__colors
            .calculator-theme__color(:class="slidein" @click="setTheme('one')")
              .color.--one
            .calculator-theme__color(:class="slidein" @click="setTheme('two')")
              .color.--two
            .calculator-theme__color(:class="slidein" @click="setTheme('three')")
              .color.-three
            .calculator-theme__color(:class="slidein" @click="setTheme('four')")
              .color.--four
            .calculator-theme__color(:class="slidein" @click="setTheme('five')")
              .color.--five
            .calculator-theme__color(:class="slidein" @click="setTheme('six')")
              .color.--six
            .calculator-theme__color(:class="slidein" @click="setTheme('seven')")
              .color.--seven
            .calculator-theme__color(:class="slidein" @click="setTheme('eight')")
              .color.--eight
            .calculator-theme__color(:class="slidein" @click="setTheme('nine')")
              .color.--nine
            .calculator-theme__color(:class="slidein" @click="setTheme('ten')")
              .color.--ten
            .calculator-theme__color(:class="slidein" @click="setTheme('eleven')")
              .color.--eleven
            .calculator-theme__color(:class="slidein" @click="setTheme('twelve')")
              .color.--twelve


        div.calculator-theme__exit
        
    //- Calculator Screen
    div.calculator-screen
      .calculator-screen-settings(@click="themeActive = !themeActive")
        svg(width='27', height='26', xmlns='http://www.w3.org/2000/svg')
          g(transform='translate(1 1)', stroke='#000', fill='none', fill-rule='evenodd', stroke-linecap='round', stroke-linejoin='round')
            ellipse(cx='22.573', cy='16.184', rx='2.895', ry='2.873')
            path(d='M22.573 19.115v4.633M22.573 0v13.31')
            ellipse(cx='12.882', cy='7.857', rx='2.895', ry='2.873')
            path(d='M12.882 10.73v13.018M12.882 0v4.984')
            ellipse(cx='3.132', cy='16.184', rx='2.895', ry='2.873')
            path(d='M3.132 19.115v4.633M3.132 0v13.31')

      .calculator-screen-history(@click="restoreHistory()") {{ history }}
      .calculator-screen-expression(:style="fontSize") {{ placeholder }} {{ expression }}

    //- Calculator Number Pad
    div.calculator-numpad
      .calculator-numpad__number.--numberLeftParen(@click="inputDigit('(')") (
      .calculator-numpad__number.--numberRightParen(@click="inputDigit(')')") )
      .calculator-numpad__number.--numberPercent(@click="backspace()")
        svg(width='43', height='30', xmlns='http://www.w3.org/2000/svg')
          g(fill='none', fill-rule='evenodd')
            g(fill='#FFF', fill-rule='nonzero')
              path(d='M41.707 14.03H12.293c-.714 0-1.293.658-1.293 1.47 0 .812.579 1.47 1.293 1.47h29.414c.714 0 1.293-.658 1.293-1.47 0-.812-.579-1.47-1.293-1.47z')
              path(d='M14.167 15.5L24.434 5.238a1.31 1.31 0 1 0-1.855-1.854L11.384 14.573a1.31 1.31 0 0 0 0 1.854L22.58 27.616a1.308 1.308 0 0 0 1.855 0 1.31 1.31 0 0 0 0-1.854L14.167 15.5z')
            path(d='M2 2.74V27.72', stroke='#FFF', stroke-width='4', stroke-linecap='square')


      .calculator-numpad__number.--number7(@mousedown="inputDigit(7); highlight()" v-bind:class="highlightAnimation") 7
      .calculator-numpad__number.--number8(@click="inputDigit(8)") 8
      .calculator-numpad__number.--number9(@click="inputDigit(9)") 9
      .calculator-numpad__number.--number4(@click="inputDigit(4)") 4
      .calculator-numpad__number.--number5(@click="inputDigit(5)") 5
      .calculator-numpad__number.--number6(@click="inputDigit(6)") 6
      .calculator-numpad__number.--number1(@click="inputDigit(1)") 1
      .calculator-numpad__number.--number2(@click="inputDigit(2)") 2
      .calculator-numpad__number.--number3(@click="inputDigit(3)") 3
      .calculator-numpad__number.--number0(@click="inputDigit(0)") 0
      .calculator-numpad__number.--numberpoint(@click="inputDigit(`.`)") .
      .calculator-numpad__number.--numberequal(@click="evaluateExpression()") =

    div.calculator-operators
      .calculator-operators__operator.--clear(@click="clearScreen()") C

      //- MINUS BUTTON
      .calculator-operators__operator(@click="inputOperator('-')")
        svg(width='36', height='4', xmlns='http://www.w3.org/2000/svg')
          path(d='M36 4H0V0h36z', fill='#FFF', fill-rule='evenodd')

      //- PLUS BUTTON
      .calculator-operators__operator(@click="inputOperator('+')")
        svg(width='33', height='33', xmlns='http://www.w3.org/2000/svg')
          path(d='M33 18.857H18.857V33h-4.714V18.857H0v-4.714h14.143V0h4.714v14.143H33z', fill='#FFF', fill-rule='nonzero')

      //- MULTIPLY BUTTON
      .calculator-operators__operator(@click="inputOperator('*')")
        svg(width='30', height='30', xmlns='http://www.w3.org/2000/svg')
          path(d='M0 27.392L2.608 30 15 17.608 27.392 30 30 27.392 17.608 15 30 2.608 27.392 0 15 12.392 2.608 0 0 2.608 12.392 15z', fill='#FFF', fill-rule='nonzero')

      //- DIVIDE BUTTON
      .calculator-operators__operator(@click="inputOperator('/')")
        svg(width='31', height='31', xmlns='http://www.w3.org/2000/svg')
          g(fill='#FFF', fill-rule='nonzero')
            path(d='M20 31h-8v-8h8zM20 8h-8V0h8zM31 20v-8H0v8z')
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      placeholder: "0",
      expression: "",
      history: "",
      highlightAnimation: "",
      currentTheme: (function() {
        if(localStorage["CurrentTheme"]) {
          return localStorage.getItem('CurrentTheme');
        } else { return "background: var(--gradient-one)" }
      }()),
      themeActive: false,
      slidein: "",
      fontSize: ``
    };
  },

  methods: {
    backspace() {
      this.checkLength();
      this.expression = this.expression.slice(0, -1);
    },

    restorePlaceholder() {
      this.placeholder = "0";
    },

    checkLength() {
      if(window.matchMedia(`(max-width:500px)`).matches && (this.expression.length < 9)) {
        this.fontSize = ` `;
      }

      if(window.matchMedia(`(max-width:500px)`).matches && (this.expression.length > 9)) {
        this.fontSize = `font-size: 3.2rem`;
      }

      if(window.matchMedia(`(max-width:500px)`).matches && (this.expression.length > 12)) {
        this.fontSize = `font-size: 2.5rem`;
      }

      if(window.matchMedia(`(max-width:500px)`).matches && (this.expression.length > 15)) {
        this.expression = this.expression.slice(0, -1);
      }

    },

    restoreFontSize() {
      this.fontSize = "";
    },

    clearPlaceholder() {
      this.placeholder = "";
    },

    restoreHistory() {
      this.clearPlaceholder();
      this.expression = this.history;
    },

    pushToHistory() {
      if (this.expression === "Error") {
        return;
      }
      this.history = this.expression;
    },

    clearScreen() {
      this.pushToHistory();
      this.restorePlaceholder();
      this.expression = ``;
      this.restoreFontSize();
    },

    inputDigit(number) {
      this.clearPlaceholder();
      this.expression += `${number}`;
      this.checkLength();
    },

    inputOperator(operator) {
      this.clearPlaceholder();
      this.expression += operator;
      this.checkLength();
    },

    evaluateExpression() {
      if (this.expression === "Error") {
        return
      }
      try {
        let someAnswer = new Function(`return ${this.expression}`);
        this.expression = someAnswer();  
        let stringExpression = this.expression.toString();
        if (stringExpression.length >= 10 && Number.isInteger(this.expression) === false) {
          let formattedAnswer = parseFloat(stringExpression).toFixed(6);
          this.expression = formattedAnswer;
        }

      } catch (error) {
        this.expression = "Error";
      }
    },

    highlight() {
      this.highlightAnimation = `highlight`;
      // this.animation = `border: 2px solid pink`;
    },

    setTheme(color) {
      this.currentTheme = `background: var(--gradient-${color})`
      localStorage.setItem("CurrentTheme", this.currentTheme);
      this.themeActive = false;
    },

    enter() {
      setTimeout(() => {
        this.slidein = "slide-in";
      }, 1)
    },

    leave() {
      this.slidein = " ";
    }
  },

  mounted() {
    [
      ...document.querySelectorAll(".calculator-numpad__number"),
      ...document.querySelectorAll(".calculator-operators__operator")
    ].forEach(value => {
      value.addEventListener("touchstart", () => {
        // value.classList.add("highlight");
        value.style.setProperty("background-color", "rgba(255, 255, 255, 0.12)");
      });
      value.addEventListener("touchmove", () => {
        // value.classList.add("highlight");
        value.style.removeProperty("background-color");
      });
      value.addEventListener("mousedown", () => {
        // value.classList.add("highlight");
        value.style.setProperty("background-color", "rgba(255, 255, 255, 0.12)");
      });
      value.addEventListener("mouseup", () => {
        value.style.removeProperty("background-color");
      });
    });

    localStorage.setItem("CurrentTheme", this.currentTheme);
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
//Animations
.highlight {
  animation: highlight;
  animation-duration: 0.5s;
  animation-timing-function: ease-in-out;
}

.slideup-enter-active,
.slideup-leave-active {
  transition: all 0.5s ease-in-out;
}

.slideup-enter,
.slideup-leave-to {
  transform: translateY(-5%);
}

.slideup-enter-to,
.slideup-leave {
  transform: translateY(0%);
}

.slidein-enter-active,
.slidein-leave-active {
  transition: transform 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
}

.slidein-enter,
.slidein-leave-to {
  transform: translateY(150%);
}

.slide-enter-to,
.slidein-leave {
  transform: translateY(0%);
}

.fadein-enter-active,
.fadein-leave-active {
  transition: opacity 0.25s linear;
}

.fadein-enter,
.fadein-leave-to {
  opacity: 0;
}

.fadein-enter-to,
.fadein-leave {
  opacity: 1;
}

@keyframes highlight {
  0% {
  }

  50% {
    background-color: rgba(255, 255, 255, 0.1);
  }
}

//Component
.calculator {
  --columns: 3fr 1fr;
  --rows: 175px calc(100vh - 175px);
  --grid-template-areas: "screen screen" "numpad operators";

  //CALCULATOR GRADIENTS
  --gradient-one: linear-gradient(to bottom, rgba(250, 178, 255, 1), rgba(25, 4, 229, 1));
  --gradient-two: linear-gradient(to bottom, rgba(17, 153, 142, 1), rgba(56, 239, 125, 1));
  --gradient-three: linear-gradient(to bottom, rgba(252, 70, 107, 1), rgba(63, 94, 251, 1));
  --gradient-four: linear-gradient(to bottom, rgba(238, 9, 121, 1), rgba(255, 106, 0, 1));
  --gradient-five: linear-gradient(to bottom, rgba(203, 45, 62, 1), rgba(239, 71, 58, 1));
  --gradient-six: linear-gradient(to bottom, rgba(22, 34, 42, 1), rgba(58, 96, 115, 1));
  --gradient-seven: linear-gradient(to bottom, rgba(20, 136, 204, 1), rgba(43, 50, 178, 1));
  --gradient-eight: linear-gradient(to bottom, rgba(35, 37, 38, 1), rgba(65, 67, 69, 1));
  --gradient-nine: linear-gradient(to bottom, rgba(254, 140, 0, 1), rgba(248, 54, 0, 1));
  --gradient-ten: linear-gradient(to bottom, rgba(0, 92, 151, 1), rgba(54, 55, 149, 1));
  --gradient-eleven: linear-gradient(to bottom, rgba(0, 0, 0, 1), rgba(67, 67, 67, 1));
  --gradient-twelve: linear-gradient(to bottom, rgba(188, 78, 156, 1), rgba(248, 7, 89, 1));

  width: 100%;
  // max-width: 575px;
  height: 100%;
  background: var(--gradient-one);
  display: grid;
  grid-template-columns: var(--columns);
  grid-template-rows: var(--rows);
  grid-template-areas: var(--grid-template-areas);
  margin: 0 auto;
  font-family: var(--font-family);
  font-weight: 300;

  & .cover {
    width: 100%;
    height: 100%;
    background-color: rgba(35, 37, 38, 0.7);
    height: 100%;
    z-index: 2;
    position: fixed;
  }

  & .calculator-overlay {
    width: 100%;
    background-color: rgba(255, 255, 255, 0.9);
    @supports (-webkit-backdrop-filter: blur(1px)) {
      -webkit-backdrop-filter: blur(3px);
      background-color: rgba(255, 255, 255, 0.8);
    }
    border-top-left-radius: 6px;
    border-top-right-radius: 6px;
    position: fixed;
    z-index: 3;
    bottom: 0;
    left: 0;
    padding: 5px 0 50px;
    display: flex;
    flex-direction: column;
    justify-content: center;

    & .calculator-theme {
      & .calculator-theme__title {
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        margin: 0;
        text-align: center;
        font-size: 2rem;
        margin: 25px 0;
        font-weight: 500;
      }

      & .calculator-theme__colors {
        --columns: repeat(4, 1fr);
        --rows: repeat(3, auto);
        --grid-gap: 20px;

        width: 100%;
        display: grid;
        grid-template-columns: var(--columns);
        grid-template-rows: var(--rows);
        grid-gap: var(--grid-gap);
        max-width: 300px;
        margin: auto;

        @media screen and (min-width: 650px) {
          max-width: 400px;
        }

        @media screen and (min-width: 1100px) {
          display: flex;
          flex-direction: row;
          justify-content: space-around;
          max-width: initial;
        }

        @for $i from 1 through 12 {
           & .calculator-theme__color:nth-child(#{$i}) {
             transition-delay: 60ms + 25ms * $i;
           }
        }

        & .calculator-theme__color {
          // $colors:
          display: flex;
          flex-direction: row;
          justify-content: center;
          align-items: center;
          transition: transform 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94), opacity 0.3s ease-in-out;
          transform: translateY(200%);
          opacity: 0.1;

          &.slide-in {
              transform: translateY(0%);
              opacity: 1;
          }

          & .color {
            width: 50px;
            height: 50px;
            background-color: palevioletred;
            border-radius: 50px;
            border: 3px solid white;
            @media screen and (min-width: 1100px) {
              width: 60px;
              height: 60px;
            }
          }

          & .color.--one {
            background-color: rgba(250, 178, 255, 0.87);
          }
          & .color.--two {
            background-color: rgba(17, 153, 142, 0.87);
          }
          & .color.--three {
            background-color: rgba(252, 70, 107, 0.87);
          }
          & .color.--four {
            background-color: rgba(238, 9, 121, 0.87);
          }
          & .color.--five {
            background-color: rgba(203, 45, 62, 0.87);
          }
          & .color.--six {
            background-color: rgba(22, 34, 42, 0.87);
          }
          & .color.--seven {
            background-color: rgba(20, 136, 204, 0.87);
          }
          & .color.--eight {
            background-color: rgba(35, 37, 38, 0.87);
          }
          & .color.--nine {
            background-color: rgba(254, 140, 0, 0.87);
          }
          & .color.--ten {
            background-color: rgba(0, 92, 151, 0.87);
          }
          & .color.--eleven {
            background-color: rgba(0, 0, 0, 0.87);
          }
          & .color.--twelve {
            background-color: rgba(188, 78, 156, 0.87);
          }
        }
      }
    }
  }

  & .calculator-screen {
    background-color: white;
    grid-area: screen;
    display: flex;
    flex-direction: column;
    position: relative;
    overflow: hidden;
    // box-shadow: inset 0px -2px 7px 0px rgba(0,0,0,.1);

    & .calculator-screen-settings {
      position: absolute;
      top: 0;
      left: 0;
      width: 50px;
      height: 50px;
      // background-color: sandybrown;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    & .calculator-screen-history {
      // background-color: steelblue;
      flex-basis: calc(25% - 10px);
      display: flex;
      justify-content: flex-end;
      align-items: flex-end;
      padding: 10px;
      font-size: 1.3rem;
      color: grey;
    }

    & .calculator-screen-expression {
      --font-size: calc(4rem + 0.5vw);


      padding: 10px;
      flex-basis: calc(75% - 10px);
      display: flex;
      justify-content: flex-end;
      align-items: flex-end;
      font-size: var(--font-size);
    }
  }

  & .calculator-numpad {
    --columns: repeat(3, 1fr);
    --rows: repeat(4, auto);

    grid-area: numpad;
    display: grid;
    grid-template-columns: var(--columns);
    grid-template-rows: var(--rows);
    box-shadow: inset 0px -8px 6px 1px rgba(0, 0, 0, 0.1);

    & .calculator-numpad__number {

      display: flex;
      flex-direction: row;
      justify-content: center;
      align-items: center;
      font-size: calc(2.3rem + 0.5vw);
      color: white;
      background-color: rgba(255, 255, 255, 0);
      transition: all 0.1s ease-in-out;
      font-weight: 500;
      transition: all 0.1s linear;
      -webkit-tap-highlight-color: rgba(0, 0, 0, 0);

      // animation: scale 1s ease-in-out;
    }
  }

  & .calculator-operators {
    // background-color: crimson;
    grid-area: operators;
    background-color: rgba(255, 255, 255, 0.3);
    background-blend-mode: overlay;
    display: flex;
    flex-direction: column;
    justify-content: center;

    & .calculator-operators__operator {
      // flex-basis: 18.85%;
      flex-basis: 20%;
      -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
      display: flex;
      justify-content: center;
      align-items: center;

      // &:hover {
      //   background-color: rgba(255, 255, 255, 0.1);
      // }
    }

    & .--clear {
      color: white;
      font-weight: 700;
      font-size: 2.3rem;
    }
  }
}
</style>
