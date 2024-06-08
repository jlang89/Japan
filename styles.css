document.addEventListener('DOMContentLoaded', () => {
  const armyIcons = document.querySelectorAll('.army-icon');
  const gameContainer = document.getElementById('game-container');
  const saveGameButton = document.getElementById('save-game');
  const loadGameButton = document.getElementById('load-game');

  // Get the elements for the existing and new territories
  const territories = {
    nagasaki: document.getElementById('nagasaki'),
    shikoku: document.getElementById('shikoku'),
    edo: document.getElementById('edo'),
    sendai: document.getElementById('sendai'),
    osaka: document.getElementById('osaka'),
    hokkaido: document.getElementById('hokkaido'),
    region1: document.getElementById('region1'),
    region2: document.getElementById('region2'),
    region3: document.getElementById('region3'),
    region4: document.getElementById('region4'),
    region5: document.getElementById('region5'),
    i: document.getElementById('i'),
    h: document.getElementById('h'),
    g: document.getElementById('g'),
    f: document.getElementById('f'),
    d: document.getElementById('d'),
    c: document.getElementById('c'),
    b: document.getElementById('b'),
    e: document.getElementById('e')
  };

  // Define player colors
  const playerColors = {
    0: 'transparent', // No owner
    1: 'rgba(255, 255, 0, 0.5)', // Yellow
    2: 'rgba(255, 0, 0, 0.5)', // Red
    3: 'rgba(0, 255, 0, 0.5)', // Green
    4: 'rgba(128, 128, 128, 0.5)', // Grey
    5: 'rgba(0, 0, 255, 0.5)', // Blue
    6: 'rgba(128, 0, 128, 0.6)' // Purple with 60% transparency
  };

  let currentPlayer = 0; // Start with no owner

  armyIcons.forEach(icon => {
    icon.addEventListener('dragstart', (e) => dragStart(e, icon, false));
  });

  gameContainer.addEventListener('dragover', dragOver);
  gameContainer.addEventListener('drop', drop);

  // Add event listeners to the territories
  Object.values(territories).forEach(territory => {
    territory.addEventListener('click', () => claimTerritory(territory));
  });

  saveGameButton.addEventListener('click', saveGame);
  loadGameButton.addEventListener('click', loadGame);

  function dragStart(e, icon, isClone) {
    const id = icon.id;
    e.dataTransfer.setData('text/plain', id);

    if (!isClone) {
      // Clone the dragged element and add it to the DOM
      const newIcon = icon.cloneNode(true);
      newIcon.id = id + '-' + new Date().getTime(); // Give it a unique ID
      newIcon.addEventListener('dragstart', (e) => dragStart(e, newIcon, true));
      newIcon.addEventListener('dblclick', removeArmy);
      document.body.appendChild(newIcon);

      // Simulate dragging the cloned icon
      const event = new DragEvent('dragstart', {
        dataTransfer: e.dataTransfer
      });
      newIcon.dispatchEvent(event);
    }
  }

  function dragOver(e) {
    e.preventDefault();
  }

  function drop(e) {
    e.preventDefault();
    const id = e.dataTransfer.getData('text');
    const draggable = document.getElementById(id);
    draggable.style.position = 'absolute';
    draggable.style.left = `${e.clientX - gameContainer.offsetLeft - draggable.width / 2}px`;
    draggable.style.top = `${e.clientY - gameContainer.offsetTop - draggable.height / 2}px`;
    gameContainer.appendChild(draggable);
  }

  function removeArmy(e) {
    e.target.remove();
  }

  function claimTerritory(territory) {
    // Cycle through player colors including no owner
    currentPlayer = (currentPlayer + 1) % 7; // Include the new color
    territory.style.backgroundColor = playerColors[currentPlayer];
  }

  // Adding double-click event listener to initial icons
  document.querySelectorAll('.army-icon').forEach(icon => {
    icon.addEventListener('dblclick', removeArmy);
  });

  function saveGame() {
    const gameState = {
      territories: {},
      playerStats: {},
      armies: []
    };

    // Save territories
    Object.keys(territories).forEach(key => {
      gameState.territories[key] = territories[key].style.backgroundColor;
    });

    // Save player stats
    ['hokkaido', 'sendai', 'shikoku', 'edo', 'osaka', 'nagasaki'].forEach(region => {
      gameState.playerStats[region] = {
        koku: document.getElementById(`${region}-koku`).value,
        samurai: document.getElementById(`${region}-samurai`).value,
        happiness: document.getElementById(`${region}-happiness`).value
      };
    });

    // Save armies
    document.querySelectorAll('.army-icon').forEach(icon => {
      if (icon.style.position === 'absolute') {
        gameState.armies.push({
          src: icon.src,
          id: icon.id,
          left: icon.style.left,
          top: icon.style.top
        });
      }
    });

    localStorage.setItem('gameState', JSON.stringify(gameState));
  }

  function loadGame() {
    const gameState = JSON.parse(localStorage.getItem('gameState'));

    if (gameState) {
      // Load territories
      Object.keys(gameState.territories).forEach(key => {
        territories[key].style.backgroundColor = gameState.territories[key];
      });

      // Load player stats
      Object.keys(gameState.playerStats).forEach(region => {
        document.getElementById(`${region}-koku`).value = gameState.playerStats[region].koku;
        document.getElementById(`${region}-samurai`).value = gameState.playerStats[region].samurai;
        document.getElementById(`${region}-happiness`).value = gameState.playerStats[region].happiness;
      });

      // Load armies
      document.querySelectorAll('.army-icon').forEach(icon => {
        if (icon.id.includes('-')) {
          icon.remove();
        }
      });

      gameState.armies.forEach(army => {
        const icon = document.createElement('img');
        icon.src = army.src;
        icon.id = army.id;
        icon.className = 'draggable army-icon';
        icon.style.position = 'absolute';
        icon.style.left = army.left;
        icon.style.top = army.top;
        icon.draggable = true;
        icon.addEventListener('dragstart', (e) => dragStart(e, icon, true));
        icon.addEventListener('dblclick', removeArmy);
        gameContainer.appendChild(icon);
      });

      // Restore click event listeners for territories
      Object.values(territories).forEach(territory => {
        territory.addEventListener('click', () => claimTerritory(territory));
      });

      // Restore event listeners for initial icons
      document.querySelectorAll('.army-icon').forEach(icon => {
        if (!icon.id.includes('-')) {
          icon.addEventListener('dragstart', (e) => dragStart(e, icon, false));
        }
      });
    }
  }
});

