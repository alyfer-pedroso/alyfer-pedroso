

<!--
**alyfer-pedroso/alyfer-pedroso** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

<div>
  <canvas width="420px" height="350px"></canvas>

  <script>
  class Player {
    constructor(x, y, width, height, speed) {
      this.x = x;
      this.y = y;
      this.width = width;
      this.height = height;
      this.speed = speed;
      this.direction = DIRECTION_RIGHT;
      this.moving = false;
      this.ignore = false;
      this.count = 0;
      // setInterval(() => this.changeRandomDirection(), 300);
    }
  
    changeRandomDirection() {
      const random = Math.floor(Math.random() * 4) + 1;
      this.direction = random;
    }
  
    teste() {
      switch (this.direction) {
        case 4:
          if (
            map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize + 0.9999 + 0.9999)] === 1 ||
            map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize + 0.9999)] === 1
          )
            this.changeRandomDirection();
          break;
        case 3:
          if (
            map[parseInt(this.y / oneBlockSize + 0.9999 + 0.9999)][parseInt(this.x / oneBlockSize)] === 1 ||
            map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize)] === 1
          )
            this.changeRandomDirection();
          break;
        case 2:
          if (
            map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize - 0.9999 - 0.9999)] === 1 ||
            map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize - 0.9999)] === 1
          )
            this.changeRandomDirection();
          break;
        case 1:
          if (
            map[parseInt(this.y / oneBlockSize - 0.9999 - 0.9999)][parseInt(this.x / oneBlockSize)] === 1 ||
            map[parseInt(this.y / oneBlockSize - 0.9999)][parseInt(this.x / oneBlockSize)] === 1
          )
            this.changeRandomDirection();
          break;
      }
    }
  
    moveProcess() {
      this.isStart();
      this.isExit();
  
      this.moving && this.lastPos();
      this.moveForwards();
  
      if (this.checkCollisions()) {
        this.moveBackwards();
        // this.count += 1;
        // setTimeout(() => this.changeRandomDirection(), 200);
        switch (this.direction) {
          case 4:
            this.direction = DIRECTION_BOTTOM;
            // setTimeout(() => (this.direction = DIRECTION_BOTTOM), 150);
            break;
          case 3:
            this.direction = DIRECTION_LEFT;
            // setTimeout(() => (this.direction = DIRECTION_LEFT), 150);
            break;
          case 2:
            this.direction = DIRECTION_UP;
            // setTimeout(() => (this.direction = DIRECTION_UP), 150);
            break;
          case 1:
            this.direction = DIRECTION_RIGHT;
            // setTimeout(() => (this.direction = DIRECTION_RIGHT), 150);
            break;
        }
      }
  
      this.checkIsAllCollisions();
  
      // if (this.count === 20) {
      //   this.count = 0;
      //   this.changeRandomDirection();
      // }
  
      // if (this.checkCollisions()) {
      //   switch (this.direction) {
      //     case 4:
      //       setTimeout(() => (this.direction = DIRECTION_BOTTOM), 200);
      //       break;
      //     case 3:
      //       setTimeout(() => (this.direction = DIRECTION_LEFT), 200);
      //       break;
      //     case 2:
      //       setTimeout(() => (this.direction = DIRECTION_UP), 200);
      //       break;
      //     case 1:
      //       setTimeout(() => (this.direction = DIRECTION_RIGHT), 200);
      //       break;
      //   }
      // }
    }
  
    isExit() {
      if (map[parseInt(this.y / oneBlockSize)][this.x / oneBlockSize] == 3) endGame = true;
    }
  
    isStart() {
      if (map[parseInt(this.y / oneBlockSize)][this.x / oneBlockSize] == 4) this.direction = DIRECTION_RIGHT;
    }
  
    moveBackwards() {
      switch (this.direction) {
        case 4: // Right
          this.x -= this.speed;
          break;
        case 1: // Up
          this.y += this.speed;
          break;
        case 2: // Left
          this.x += this.speed;
          break;
        case 3: // Bottom
          this.y -= this.speed;
          break;
      }
      this.moving = false;
    }
  
    moveForwards() {
      switch (this.direction) {
        case 4: // Right
          this.x += this.speed;
          break;
        case 1: // Up
          this.y -= this.speed;
          break;
        case 2: // Left
          this.x -= this.speed;
          break;
        case 3: // Bottom
          this.y += this.speed;
          break;
      }
      this.moving = true;
    }
  
    lastPos() {
      let lastX = parseInt(this.x / oneBlockSize);
      let lastY = parseInt(this.y / oneBlockSize + 0.9999);
      setTimeout(() => {
        if (map[lastY][lastX] !== 3 && map[lastY][lastX] !== 4) {
          map[lastY][lastX] = 2;
        }
      }, 200);
    }
  
    checkCollisions() {
      let isCollided = false;
      if (
        map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize)] == 1 ||
        map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize)] == 1 ||
        map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize + 0.9999)] == 1 ||
        map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize + 0.9999)] == 1 ||
        (map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize)] == 2 && !this.ignore) ||
        (map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize)] == 2 && !this.ignore) ||
        (map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize + 0.9999)] == 2 && !this.ignore) ||
        (map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize + 0.9999)] == 2 && !this.ignore)
      ) {
        isCollided = true;
      }
      this.ignore = false;
      return isCollided;
    }
  
    resetLastPos() {
      for (let i = 0; i < map.length; i++) {
        for (let j = 0; j < map[0].length; j++) {
          if (map[i][j] == 2) {
            map[i][j] = 0;
          }
        }
      }
    }
  
    checkIsAllCollisions() {
      if (
        (map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize)] == 1 ||
          map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize)] == 2) &&
        (map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize)] == 1 ||
          map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize)] == 2) &&
        (map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize + 0.9999)] == 1 ||
          map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize + 0.9999)] == 2) &&
        (map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize + 0.9999)] == 1 ||
          map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize + 0.9999)] == 2)
      ) {
        this.ignore = true;
        // this.resetLastPos();
      }
    }
  
    draw() {
      createRect(this.x, this.y, this.width, this.height, "red");
    }
  
    update() {
      // this.teste();
      this.moveProcess();
      this.draw();
    }
  }
  
  class Player2 {
    constructor(x, y, width, height, speed) {
      this.x = x;
      this.y = y;
      this.width = width;
      this.height = height;
      this.speed = speed;
      this.direction = DIRECTION_RIGHT;
      this.moving = false;
      this.ignore = true;
      this.count = 0;
      setInterval(() => this.changeRandomDirection(), 300);
    }
  
    changeRandomDirection() {
      const random = Math.floor(Math.random() * 4) + 1;
      this.direction = random;
    }
  
    teste() {
      switch (this.direction) {
        case 4:
          if (
            map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize + 0.9999 + 0.9999)] === 1 ||
            map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize + 0.9999)] === 1
          )
            this.changeRandomDirection();
          break;
        case 3:
          if (
            map[parseInt(this.y / oneBlockSize + 0.9999 + 0.9999)][parseInt(this.x / oneBlockSize)] === 1 ||
            map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize)] === 1
          )
            this.changeRandomDirection();
          break;
        case 2:
          if (
            map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize - 0.9999 - 0.9999)] === 1 ||
            map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize - 0.9999)] === 1
          )
            this.changeRandomDirection();
          break;
        case 1:
          if (
            map[parseInt(this.y / oneBlockSize - 0.9999 - 0.9999)][parseInt(this.x / oneBlockSize)] === 1 ||
            map[parseInt(this.y / oneBlockSize - 0.9999)][parseInt(this.x / oneBlockSize)] === 1
          )
            this.changeRandomDirection();
          break;
      }
    }
  
    moveProcess() {
      this.isStart();
      this.isExit();
      this.moving && this.lastPos();
      this.moveForwards();
      if (this.checkCollisions()) this.moveBackwards();
    }
  
    isExit() {
      if (map[parseInt(this.y / oneBlockSize)][this.x / oneBlockSize] == 3) endGame = true;
    }
  
    isStart() {
      if (map[parseInt(this.y / oneBlockSize)][this.x / oneBlockSize] == 4) this.direction = DIRECTION_RIGHT;
    }
  
    moveBackwards() {
      switch (this.direction) {
        case 4: // Right
          this.x -= this.speed;
          break;
        case 1: // Up
          this.y += this.speed;
          break;
        case 2: // Left
          this.x += this.speed;
          break;
        case 3: // Bottom
          this.y -= this.speed;
          break;
      }
      this.moving = false;
    }
  
    moveForwards() {
      switch (this.direction) {
        case 4: // Right
          this.x += this.speed;
          break;
        case 1: // Up
          this.y -= this.speed;
          break;
        case 2: // Left
          this.x -= this.speed;
          break;
        case 3: // Bottom
          this.y += this.speed;
          break;
      }
      this.moving = true;
    }
  
    lastPos() {
      let lastX = parseInt(this.x / oneBlockSize);
      let lastY = parseInt(this.y / oneBlockSize + 0.9999);
      setTimeout(() => {
        if (map[lastY][lastX] !== 3 && map[lastY][lastX] !== 4) {
          map[lastY][lastX] = 2;
        }
      }, 200);
    }
  
    checkCollisions() {
      let isCollided = false;
      if (
        map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize)] == 1 ||
        map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize)] == 1 ||
        map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize + 0.9999)] == 1 ||
        map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize + 0.9999)] == 1 ||
        (map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize)] == 2 && !this.ignore) ||
        (map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize)] == 2 && !this.ignore) ||
        (map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize + 0.9999)] == 2 && !this.ignore) ||
        (map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize + 0.9999)] == 2 && !this.ignore)
      ) {
        isCollided = true;
      }
      // this.ignore = false;
      return isCollided;
    }
  
    resetLastPos() {
      for (let i = 0; i < map.length; i++) {
        for (let j = 0; j < map[0].length; j++) {
          if (map[i][j] == 2) {
            map[i][j] = 0;
          }
        }
      }
    }
  
    checkIsAllCollisions() {
      if (
        (map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize)] == 1 ||
          map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize)] == 2) &&
        (map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize)] == 1 ||
          map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize)] == 2) &&
        (map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize + 0.9999)] == 1 ||
          map[parseInt(this.y / oneBlockSize)][parseInt(this.x / oneBlockSize + 0.9999)] == 2) &&
        (map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize + 0.9999)] == 1 ||
          map[parseInt(this.y / oneBlockSize + 0.9999)][parseInt(this.x / oneBlockSize + 0.9999)] == 2)
      ) {
        this.ignore = true;
        // this.resetLastPos();
      }
    }
  
    draw() {
      createRect(this.x, this.y, this.width, this.height, "red");
    }
  
    update() {
      // this.teste();
      this.moveProcess();
      this.draw();
    }
  }
    
  const canvas = document.querySelector("canvas");
  const ctx = canvas.getContext("2d");
  
  const DIRECTION_RIGHT = 4;
  const DIRECTION_BOTTOM = 3;
  const DIRECTION_LEFT = 2;
  const DIRECTION_UP = 1;
  const oneBlockSize = 35;
  const wallSpaceWidth = oneBlockSize / 1.6;
  const wallOffset = (oneBlockSize - wallSpaceWidth) / 2;
  const wallInnerColor = "black";
  let endGame = false;
  let count = 0;
  
  const random = Math.floor(Math.random() * 4) + 1;
  
  const map =
    random > 2
      ? [
          [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
          [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
          [1, 0, 0, 1, 0, 0, 0, 1, 1, 1, 0, 1],
          [1, 1, 1, 1, 0, 1, 0, 0, 0, 1, 1, 1],
          [1, 0, 0, 0, 0, 1, 1, 0, 0, 1, 0, 1],
          [1, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 1],
          [4, 0, 0, 1, 1, 1, 0, 0, 0, 1, 1, 1],
          [1, 0, 1, 1, 0, 0, 0, 1, 1, 1, 1, 1],
          [1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 3],
          [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
        ]
      : [
          [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
          [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
          [1, 0, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1],
          [1, 0, 1, 1, 0, 1, 0, 0, 0, 1, 1, 1],
          [1, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 1],
          [1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1],
          [4, 0, 1, 0, 0, 0, 0, 0, 0, 1, 1, 1],
          [1, 0, 1, 0, 1, 1, 0, 1, 1, 1, 0, 1],
          [1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 3],
          [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
        ];
  
  const createRect = (x, y, width, height, color) => {
    ctx.fillStyle = color;
    ctx.fillRect(x, y, width, height);
  };
  
  const drawWalls = () => {
    for (let i = 0; i < map.length; i++) {
      for (let j = 0; j < map[0].length; j++) {
        if (map[i][j] == 1) {
          createRect(j * oneBlockSize, i * oneBlockSize, oneBlockSize, oneBlockSize, "#3AADCA");
          if (j > 0 && map[i][j - 1] == 1) {
            createRect(j * oneBlockSize, i * oneBlockSize + wallOffset, wallSpaceWidth + wallOffset, wallSpaceWidth, wallInnerColor);
          }
  
          if (j < map[0].length - 1 && map[i][j + 1] == 1) {
            createRect(j * oneBlockSize + wallOffset, i * oneBlockSize + wallOffset, wallSpaceWidth + wallOffset, wallSpaceWidth, wallInnerColor);
          }
  
          if (i < map.length - 1 && map[i + 1][j] == 1) {
            createRect(j * oneBlockSize + wallOffset, i * oneBlockSize + wallOffset, wallSpaceWidth, wallSpaceWidth + wallOffset, wallInnerColor);
          }
  
          if (i > 0 && map[i - 1][j] == 1) {
            createRect(j * oneBlockSize + wallOffset, i * oneBlockSize, wallSpaceWidth, wallSpaceWidth + wallOffset, wallInnerColor);
          }
        }
        if (map[i][j] == 2) {
          createRect(j * oneBlockSize + oneBlockSize / 3, i * oneBlockSize + oneBlockSize / 3, oneBlockSize / 3, oneBlockSize / 3, "#FEB897");
        }
      }
    }
  };
  
  // let player = new Player(0, oneBlockSize * 6, oneBlockSize, oneBlockSize, oneBlockSize / 5);
  let player = new Player2(0, oneBlockSize * 6, oneBlockSize, oneBlockSize, oneBlockSize / 5);
  // let player = new Player(0 + 5, 5 + oneBlockSize * 6, oneBlockSize - 10, oneBlockSize - 10, oneBlockSize / 7);
  
  const gameLoop = () => {
    if (endGame) {
      clearInterval(gameInterval);
      alert("Fim de jogo! Você saiu do labirinto");
    }
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    createRect(0, 0, canvas.width, canvas.height, "black");
  
    drawWalls();
    player.update();
  };
  
  const start = () => {
    // setTimeout(() => {
    //   map[6][0] = 1;
    // }, 300);
  };
  
  start();
  gameInterval = setInterval(gameLoop, 1000 / 30);
</script>
</div>
