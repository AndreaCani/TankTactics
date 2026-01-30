# Tank Tactics - Guida alle Modifiche

## 📋 Panoramica

Tank Tactics è un gioco di strategia in tempo reale completamente personalizzabile. Questa guida ti spiega come modificare unità, edifici, livelli e molto altro.

## 🎮 Come Giocare

- **WASD o Frecce**: Muovi l'unità
- **Click sinistro**: Spara verso il cursore
- **Obiettivo**: Conquista la base nemica o elimina tutte le truppe nemiche
- **Conquista**: Rimani sopra un edificio nemico/neutrale per catturarlo (barra di progresso)
- **Respawn**: Dopo la morte, puoi comprare una nuova unità se hai abbastanza soldi
- **Ospedale**: Conquistarlo cambia il tuo punto di respawn

## 🔧 Struttura del Codice

Il gioco è contenuto in un singolo file HTML (`tank-tactics.html`) con JavaScript integrato. Le configurazioni sono organizzate in oggetti JavaScript facilmente modificabili.

### Sezioni Principali nel Codice

1. **UNIT_TYPES** (linea ~80): Definizione di tutte le unità
2. **BUILDING_TYPES** (linea ~140): Definizione di tutti gli edifici
3. **LEVELS** (linea ~190): Definizione di tutti i livelli

## ⚔️ Modificare Unità Esistenti

Trova l'oggetto `UNIT_TYPES` nel codice e modifica i valori:

```javascript
soldier: {
    name: 'Soldato',
    cost: 100,              // Costo in denaro
    speed: 2,               // Velocità di movimento (pixel per frame)
    health: 50,             // Punti vita
    maxHealth: 50,          // Vita massima (per calcolo barra)
    damage: 10,             // Danno per colpo
    captureSpeed: 1,        // Velocità di conquista (1 = normale, <1 = lento)
    fireRate: 500,          // Millisecondi tra un colpo e l'altro
    bulletSpeed: 5,         // Velocità proiettile
    color: '#4a8cdc'        // Colore dell'unità (esadecimale)
}
```

## ➕ Aggiungere Nuove Unità

Aggiungi una nuova entry nell'oggetto `UNIT_TYPES`:

```javascript
UNIT_TYPES = {
    // ... unità esistenti ...
    
    sniper: {
        name: 'Cecchino',
        cost: 350,
        speed: 2.5,
        health: 60,
        maxHealth: 60,
        damage: 80,              // Alto danno
        captureSpeed: 0.8,
        fireRate: 1500,           // Lento a sparare
        bulletSpeed: 10,          // Proiettile molto veloce
        color: '#8e44ad'
    },
    
    medic: {
        name: 'Medico',
        cost: 250,
        speed: 3,
        health: 70,
        maxHealth: 70,
        damage: 5,                // Basso danno
        captureSpeed: 1.2,        // Buono a conquistare
        fireRate: 600,
        bulletSpeed: 4,
        color: '#16a085'
    }
}
```

## 🏢 Modificare Edifici Esistenti

Trova l'oggetto `BUILDING_TYPES` e modifica:

```javascript
house: {
    name: 'Casa',
    incomePerSecond: 5,         // Denaro generato al secondo
    captureTime: 3000,          // Millisecondi per conquistare
    color: '#7a7a7a',           // Colore edificio
    size: 2                     // Dimensione in celle (2x2, 3x3, etc.)
}
```

## 🏗️ Aggiungere Nuovi Tipi di Edifici

```javascript
BUILDING_TYPES = {
    // ... edifici esistenti ...
    
    factory: {
        name: 'Fabbrica',
        incomePerSecond: 15,     // Genera più soldi
        captureTime: 6000,       // Difficile da conquistare
        color: '#34495e',
        size: 3
    },
    
    fortress: {
        name: 'Fortezza',
        incomePerSecond: 0,
        captureTime: 8000,       // Molto difficile da conquistare
        color: '#2c3e50',
        size: 4,
        defensiveBonus: true     // Puoi aggiungere proprietà custom
    }
}
```

## 🗺️ Creare Nuovi Livelli

Aggiungi una nuova entry nell'array `LEVELS`:

```javascript
{
    id: 4,                           // ID univoco progressivo
    name: 'Livello 4 - Nome Livello',
    gridWidth: 35,                   // Larghezza griglia
    gridHeight: 25,                  // Altezza griglia
    startMoney: 1000,                // Soldi iniziali
    fogOfWar: true,                  // true/false per nebbia di guerra
    playerStart: { x: 3, y: 12 },    // Posizione spawn (non usata ora)
    
    buildings: [
        // Base del giocatore (OBBLIGATORIA)
        { type: 'base', x: 2, y: 12, owner: 'player' },
        
        // Edifici neutrali
        { type: 'house', x: 10, y: 10, owner: 'neutral' },
        { type: 'barracks', x: 15, y: 12, owner: 'neutral' },
        { type: 'hospital', x: 20, y: 12, owner: 'neutral' },
        
        // Base nemica (OBBLIGATORIA)
        { type: 'base', x: 32, y: 12, owner: 'enemy' },
        
        // Edifici nemici (opzionali)
        { type: 'barracks', x: 28, y: 10, owner: 'enemy' }
    ],
    
    enemies: [
        // Ogni nemico ha un numero massimo di spawn
        { type: 'soldier', x: 30, y: 10, maxSpawns: 4 },
        { type: 'jeep', x: 30, y: 14, maxSpawns: 3 },
        { type: 'tank', x: 32, y: 12, maxSpawns: 2 }
    ],
    
    obstacles: [
        // Ostacoli sono aree invalicabili
        { x: 15, y: 5, width: 3, height: 5 },
        { x: 15, y: 15, width: 3, height: 5 }
    ]
}
```

## 📍 Posizionamento Edifici

Le coordinate `x` e `y` sono in unità griglia (non pixel):
- Un edificio con `size: 2` occupa una area 2x2
- La posizione (x, y) è l'angolo in alto a sinistra
- Assicurati che gli edifici non si sovrappongano

**Esempio di calcolo:**
```javascript
// Edificio size 3 alla posizione (5, 10)
// Occupa le celle: (5,10), (6,10), (7,10), (5,11), (6,11), (7,11), (5,12), (6,12), (7,12)
```

## 🎯 Best Practices per i Livelli

### Livello Facile
- Grid: 30x20
- Soldi iniziali: 1000-1500
- Nemici: 2-3 tipi, maxSpawns bassi (2-4)
- Edifici: 3-5, pochi neutrali
- Fog of war: No

### Livello Medio
- Grid: 35x25
- Soldi iniziali: 800-1200
- Nemici: 3-4 tipi, maxSpawns medi (3-5)
- Edifici: 5-8, mix neutral/enemy
- Fog of war: Opzionale
- Aggiungi ostacoli per complessità

### Livello Difficile
- Grid: 40x30+
- Soldi iniziali: 600-1000
- Nemici: 4-5 tipi, maxSpawns alti (4-7)
- Edifici: 8-12, molti enemy-owned
- Fog of war: Sì
- Molti ostacoli e caserme nemiche

## 🎨 Personalizzazione Visiva

### Cambiare Colori Tema

Trova le variabili CSS all'inizio del file:

```css
:root {
    --military-dark: #1a2a1a;      /* Sfondo scuro principale */
    --military-green: #2d4a2d;     /* Verde militare */
    --military-olive: #4a6b3a;     /* Oliva per gradienti */
    --ui-gold: #d4af37;            /* Oro per UI/bordi */
    --enemy-red: #c42c2c;          /* Rosso nemici */
    --ally-blue: #2c5c9c;          /* Blu alleati */
    --neutral-gray: #7a7a7a;       /* Grigio neutrali */
}
```

### Dimensioni Griglia

```javascript
const GRID_SIZE = 40;    // Numero di celle (non usato attualmente)
const TILE_SIZE = 40;    // Dimensione in pixel di ogni cella
```

Riduci `TILE_SIZE` a 30 o 35 per mappe più grandi visibili a schermo.

## 🔨 Meccaniche Avanzate

### Aggiungere Abilità Speciali

Puoi estendere le unità con proprietà custom e implementare logica nel metodo `update()`:

```javascript
// In UNIT_TYPES
healer: {
    name: 'Guaritore',
    // ... altre proprietà ...
    healRate: 2000,          // Proprietà custom
    healAmount: 20
}

// Nel metodo Unit.update(), aggiungi:
if (this.type === 'healer' && Date.now() - this.lastHeal > this.healRate) {
    // Logica per curare unità vicine
    this.lastHeal = Date.now();
}
```

### Power-Ups dall'Aeroporto

L'aeroporto ha già il parametro `powerUpInterval`. Puoi implementare la logica aggiungendo nel ciclo di update:

```javascript
// Nella funzione updateGame, sezione edifici
if (building.type === 'airport' && building.owner === 'player') {
    if (now - building.lastPowerUp > building.powerUpInterval) {
        // Spawna power-up
        gameState.powerUps.push({
            x: building.x * TILE_SIZE,
            y: building.y * TILE_SIZE,
            type: 'speed' // o 'damage', 'health', etc.
        });
        building.lastPowerUp = now;
    }
}
```

## 🐛 Debug e Testing

### Console del Browser

Apri la console (F12) mentre giochi per vedere errori e debug:

```javascript
// Aggiungi log nel codice
console.log('Player position:', gameState.player.x, gameState.player.y);
console.log('Money:', gameState.money);
console.log('Enemies:', gameState.enemies.length);
```

### Comandi Cheat (Opzionale)

Aggiungi questi nel codice per testare:

```javascript
// Dopo la definizione di gameState, aggiungi:
window.cheat = {
    addMoney: (amount) => { gameState.money += amount; updateUI(); },
    killAllEnemies: () => { gameState.enemies = []; },
    godMode: () => { 
        if (gameState.player) gameState.player.health = 99999; 
    }
};

// Usa nella console: cheat.addMoney(5000)
```

## 📝 Esempi Pratici

### Esempio 1: Livello "Assedio"
Giocatore deve difendere la base mentre ondate nemiche attaccano:

```javascript
{
    id: 5,
    name: 'Assedio',
    gridWidth: 30,
    gridHeight: 30,
    startMoney: 2000,
    fogOfWar: false,
    playerStart: { x: 15, y: 15 },
    buildings: [
        { type: 'base', x: 14, y: 14, owner: 'player' },
        { type: 'hospital', x: 12, y: 12, owner: 'player' },
        { type: 'barracks', x: 3, y: 3, owner: 'enemy' },
        { type: 'barracks', x: 26, y: 3, owner: 'enemy' },
        { type: 'barracks', x: 3, y: 26, owner: 'enemy' },
        { type: 'barracks', x: 26, y: 26, owner: 'enemy' }
    ],
    enemies: [
        { type: 'soldier', x: 5, y: 5, maxSpawns: 10 },
        { type: 'soldier', x: 24, y: 5, maxSpawns: 10 },
        { type: 'jeep', x: 5, y: 24, maxSpawns: 8 },
        { type: 'tank', x: 24, y: 24, maxSpawns: 5 }
    ],
    obstacles: []
}
```

### Esempio 2: Unità "Commando" Veloce

```javascript
// In UNIT_TYPES
commando: {
    name: 'Commando',
    cost: 600,
    speed: 6,              // Molto veloce
    health: 100,
    maxHealth: 100,
    damage: 25,
    captureSpeed: 1.5,     // Eccellente per conquistare
    fireRate: 350,         // Spara rapidamente
    bulletSpeed: 8,
    color: '#2ecc71'
}
```

## 💡 Suggerimenti

1. **Bilanciamento**: Testa ogni livello più volte
2. **Progressione**: Aumenta gradualmente la difficoltà
3. **Varietà**: Usa combinazioni diverse di edifici ed nemici
4. **Economia**: Considera quanto denaro serve per comprare unità forti
5. **Fog of War**: Usalo per livelli più tattici e lenti
6. **Ostacoli**: Creano percorsi forzati e coperture

## 🚀 Prossimi Passi

Ora che conosci la struttura, puoi:
- Creare nuove unità con abilità uniche
- Progettare livelli elaborati con layout complessi
- Sperimentare con meccaniche custom
- Modificare l'estetica e i colori
- Aggiungere suoni ed effetti (richiede file audio)

Buon divertimento con Tank Tactics! 🎮
