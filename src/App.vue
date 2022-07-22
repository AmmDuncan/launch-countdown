<template>
  <div class="page">
    <section class="content">
      <h1>We're launching soon</h1>

      <div class="board">
        <!-- DAYS -->
        <div class="card" data-label="Days" ref="day">
          <div class="top">
            <div class="upper"></div>
            <div class="behind"></div>
          </div>
          <div class="lower"></div>
          <div class="value">
            <span class="inner">{{ String(days).padStart(2, "0") }}</span>
          </div>
        </div>

        <!-- HOURS -->
        <div class="card" data-label="Hours" ref="hour">
          <div class="top">
            <div class="upper"></div>
            <div class="behind"></div>
          </div>
          <div class="lower"></div>
          <div class="value">
            <span class="inner">{{ hoursLeft }}</span>
          </div>
        </div>

        <!-- MINUTES -->
        <div class="card" data-label="Minutes" ref="minute">
          <div class="top">
            <div class="upper"></div>
            <div class="behind"></div>
          </div>
          <div class="lower"></div>
          <div class="value">
            <span class="inner">{{ minutesLeft }}</span>
          </div>
        </div>

        <!-- SECONDS -->
        <div class="card" data-label="Seconds" ref="second">
          <div class="top">
            <div class="upper"></div>
            <div class="behind"></div>
          </div>
          <div class="lower"></div>
          <div class="value">
            <span class="inner">{{ secondsLeft }}</span>
          </div>
        </div>
      </div>
    </section>

    <figure class="illustration">
      <img src="./assets/pattern-hills.svg" alt="" />
    </figure>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data: () => ({
    time: 60,
    date: new Date(2022, 9, 28),
    now: new Date(),
  }),
  computed: {
    timestamp() {
      return this.date.getTime() - this.now.getTime();
    },
    seconds() {
      return Math.floor(this.timestamp / 1000);
    },
    minutes() {
      return Math.floor(this.seconds / 60);
    },
    hours() {
      return Math.floor(this.minutes / 60);
    },
    days() {
      const days = Math.floor(this.hours / 24);
      return days;
    },
    hoursLeft() {
      return String(Math.floor(this.hours - this.days * 24)).padStart(2, "0");
    },
    minutesLeft() {
      return String(Math.floor(this.minutes - this.hours * 60)).padStart(
        2,
        "0"
      );
    },
    secondsLeft() {
      // this.$refs.second?.classList.add("animate");
      return String(Math.floor(this.seconds - this.minutes * 60)).padStart(
        2,
        "0"
      );
    },
  },
  mounted() {
    setInterval(() => {
      this.now = new Date();
    }, 1000);
  },
  watch: {
    // secondsLeft() {
    //   const v = this.$refs.second.querySelector(".value");
    //   v.style.opacity = 0;
    //   setTimeout(() => {
    //     this.$refs.second?.classList.remove("animate");
    //     v.style.opacity = 1;
    //   }, 500);
    // },
  },
};
</script>

<style lang="scss">
@import "./styles/main";

#app {
  background: $darkest-blue;
}

.page {
  min-height: 100vh;
  min-width: 100vw;
  display: flex;
  flex-flow: column nowrap;
  justify-content: space-between;
  background: url("./assets/bg-stars.svg");
  background-size: cover;
}

.content {
  width: 80%;
  margin: auto;

  h1 {
    font-size: 2.4rem;
    font-weight: 700;
    text-transform: uppercase;
    text-align: center;
    letter-spacing: 0.2em;
  }
}

.board {
  display: grid;
  place-content: center;
  margin-top: 8rem;
  grid: 1fr / auto-flow auto;
  gap: 4rem;
}

.card {
  --height: 12rem;
  --half-height-main: calc((var(--height) / 2) * 0.93);
  --half-height-rem: calc((var(--height) / 2) * 0.07);
  --animation-duration: 0.75s;
  @include flex-column;
  position: relative;
  height: var(--height);
  width: 13rem;
  justify-content: space-between;
  perspective: 400px;
  &:before {
    @include show-pseudo;
    z-index: 11;
    height: 1rem;
    background: transparent;
    box-shadow: 0 -0.4rem 0.8rem -0.2rem rgba($darker-blue, 0.2) inset;
    border-bottom: 1px solid rgba($darker-blue, 0.15);
    bottom: 50%;
    left: 0;
  }
  &:after {
    @include show-pseudo;
    content: attr(data-label);
    color: $greyish-blue;
    width: max-content;
    left: 50%;
    bottom: -4rem;
    transform: translateX(-50%);
    font-size: 1.8rem;
    font-weight: 600;
    letter-spacing: 0.25em;
    text-transform: uppercase;
  }
  .lower {
    position: relative;
    height: var(--half-height-main);
    background: $dark-blue;
    border-radius: 0.4rem;
    box-shadow: 0 0.8rem 0.4rem darken($darkest-blue, 3%);
    &:after {
      @include show-pseudo;
      top: calc(var(--half-height-rem) * -1);
      height: var(--half-height-rem);
      width: 90%;
      left: 50%;
      transform: translateX(-50%);
      background: inherit;
    }
  }

  .top {
    background: transparent;
    position: relative;

    > * {
      position: absolute;
      height: var(--half-height-main);
      border-radius: 0.4rem;
      width: 100%;
      background: darken($dark-blue, 5%);
      transform-style: preserve-3d;
      &:after {
        @include show-pseudo;
        height: var(--half-height-rem);
        width: 90%;
        left: 50%;
        transform: translateX(-50%);
        bottom: calc(var(--half-height-rem) * -1);
        background: darken($dark-blue, 5%);
      }
    }
  }

  .value {
    position: absolute;
    height: max-content;
    font-size: 7rem;
    font-weight: 700;
    top: 50%;
    left: 50%;
    transform: translateX(-50%);
    background: transparent;
    color: $soft-red;
    opacity: 0.9;

    .inner {
      position: relative;
      top: calc(var(--height) / -3.25);
    }
  }

  &.animate {
    .upper {
      animation: fold var(--animation-duration) cubic-bezier(0.25, 0.8, 0.3, 1)
        forwards infinite;
      transform-origin: 0 calc(100% + var(--half-height-rem));
    }

    .value {
      animation: cutoff calc(var(--animation-duration) * 0.25) linear;
      animation-delay: calc((var(--animation-duration) / 2) * 0.25);
      animation-fill-mode: forwards;
    }
  }
}

.illustration {
  width: 100%;
  height: max-content;
  margin-bottom: -0.2rem;

  img {
    width: 100%;
  }
}

@keyframes fold {
  from {
    transform: rotateX(0);
  }
  to {
    transform: rotateX(-180deg);
    background: darken($dark-blue, 2%);
    z-index: 15;
  }
}

@keyframes cutoff {
  0% {
    overflow: hidden;
  }
  100% {
    overflow: auto;
  }
}
</style>
