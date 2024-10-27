<template>
    <div class="pomodoro-container">
      <h1>ポモドーロタイマー</h1>
      
      <!-- タイマー表示部分 -->
      <div class="timer">
        {{ minutes }}:{{ seconds < 10 ? '0' : '' }}{{ seconds }}
      </div>
  
      <!-- コントロールボタン -->
      <div class="controls">
        <button @click="startTimer" v-if="!isRunning">スタート</button>
        <button @click="pauseTimer" v-if="isRunning">一時停止</button>
        <button @click="resetTimer">リセット</button>
      </div>
  
      <!-- 現在のモード表示 -->
      <div class="mode">
        {{ isWorkTime ? '作業中' : '休憩中' }}
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        minutes: 25,      // 残り分数
        seconds: 0,       // 残り秒数
        isRunning: false, // タイマーが動いているかどうか
        isWorkTime: true, // 作業時間か休憩時間か
        timer: null,      // タイマーのID
      }
    },
  
    methods: {
      // タイマーをスタートする関数
      startTimer() {
        this.isRunning = true
        this.timer = setInterval(() => {
          if (this.seconds === 0) {
            if (this.minutes === 0) {
              // タイマー終了時の処理
              this.timerComplete()
            } else {
              this.minutes--
              this.seconds = 59
            }
          } else {
            this.seconds--
          }
        }, 1000)
      },
  
      // タイマーを一時停止する関数
      pauseTimer() {
        this.isRunning = false
        clearInterval(this.timer)
      },
  
      // タイマーをリセットする関数
      resetTimer() {
        this.pauseTimer()
        this.minutes = this.isWorkTime ? 25 : 5
        this.seconds = 0
      },
  
      // タイマー完了時の処理
      timerComplete() {
        this.pauseTimer()
        this.isWorkTime = !this.isWorkTime
        this.minutes = this.isWorkTime ? 25 : 5
        this.seconds = 0
        alert(this.isWorkTime ? '休憩終了！作業を始めましょう！' : '作業終了！休憩しましょう！')
      }
    }
  }
  </script>
  
  <style scoped>
  .pomodoro-container {
    text-align: center;
    padding: 20px;
  }
  
  .timer {
    font-size: 48px;
    margin: 20px 0;
  }
  
  .controls button {
    margin: 0 10px;
    padding: 10px 20px;
    font-size: 16px;
  }
  
  .mode {
    margin-top: 20px;
    font-size: 20px;
  }
  </style>