# Ping Pong Game 🏓

A classic two-player Ping Pong game implemented in x86 Assembly Language (NASM). This retro console-based game demonstrates low-level programming concepts, BIOS interrupts, and real-time gameplay mechanics.

## 🎯 Overview

**Ping Pong Game** is a faithful recreation of the classic Pong arcade game, written entirely in x86 Assembly Language. The game features real-time two-player gameplay with smooth ball physics, collision detection, and score tracking—all implemented using direct video memory manipulation and BIOS interrupts.

### Game Objective

- **Player 1 (Left)**: Prevent the ball from touching the left edge
- **Player 2 (Right)**: Prevent the ball from touching the right edge
- **Win Condition**: Opponent fails to return the ball
- **Challenge**: Ball changes direction based on where the paddle hits it

### Built With

- **Language**: x86 Assembly Language (NASM/TASM)
- **Architecture**: 16-bit Real Mode
- **Platform**: DOS/DOSBox
- **Video Mode**: Text Mode (80x25) with direct video memory access

## ✨ Game Features

### Core Gameplay
- ✅ **Two-Player Mode**: Simultaneous left and right paddle control
- ✅ **Real-Time Movement**: Smooth paddle and ball animation
- ✅ **Dynamic Ball Physics**: Ball direction changes based on paddle hit position
- ✅ **Collision Detection**: Accurate detection for paddles, walls, and boundaries
- ✅ **Score Tracking**: Real-time score updates for both players
- ✅ **Game Over Screen**: Displays winner and final scores

### Visual Elements
- 🎨 **Color-Coded Display**: Different colors for players, ball, and UI
- 📏 **Center Line**: Visual divider between player territories
- 🔲 **Borders**: Top and bottom walls that reflect the ball
- 🎮 **Clean UI**: Welcome screen, instructions, and game menu

### Ball Physics
- **Straight Movement**: Direct horizontal trajectory
- **Diagonal Up**: Ball moves upward at an angle
- **Diagonal Down**: Ball moves downward at an angle
- **5 Hit Zones**: Different paddle positions create different ball angles

## 🎓 Assembly Concepts Demonstrated

This project showcases comprehensive knowledge of Assembly Language programming:

### 1. **Direct Video Memory Access**
```assembly
mov ax, 0xb800      ; Video memory segment
mov es, ax           ; Extra segment = video memory
mov [es:di], ax      ; Write directly to screen
```
- Direct manipulation of video buffer at `0xB800:0000`
- Character and attribute byte control
- Efficient screen updates without BIOS overhead

### 2. **BIOS Interrupts**
```assembly
int 0x10    ; Video services
int 0x16    ; Keyboard services
int 0x21    ; DOS services
```
- **INT 10h**: Text display, cursor positioning
- **INT 16h**: Keyboard input and scan codes
- **INT 21h**: Program termination and DOS functions

### 3. **Stack Operations**
```assembly
push bp
mov bp, sp
sub sp, 10    ; Local variables
; ... function body ...
mov sp, bp
pop bp
ret
```
- Stack frame management for subroutines
- Parameter passing via stack
- Local variable allocation

### 4. **Subroutines & Procedures**
- Modular code organization with 25+ subroutines
- Parameter passing conventions
- Return value handling
- Proper register preservation

### 5. **Addressing Modes**
- **Immediate**: `mov ax, 0xb800`
- **Direct**: `mov ax, [score1]`
- **Register Indirect**: `mov [es:di], ax`
- **Based**: `mov ax, [bp+4]`

### 6. **Conditional Jumps & Loops**
```assembly
cmp si, 4000
jl ballMovementDiagonallyDown1
je rightCollisionTrue
jne secondP2
```
- Signed and unsigned comparisons
- Short and near jumps
- Loop constructs with `loop` instruction

### 7. **Real-Time Input Handling**
```assembly
mov ah, 1
int 0x16        ; Check for keystroke (non-blocking)
jz exitInput    ; Jump if no key pressed
```
- Non-blocking keyboard checks
- Scan code processing
- Real-time game control

### 8. **String Operations**
```assembly
cld             ; Clear direction flag
rep stosw       ; Repeat store word
```
- Direction flag manipulation
- String repeat operations
- Efficient memory filling

## 🎮 Game Mechanics

### Player Controls

| Player | Move Up | Move Down |
|--------|---------|-----------|
| **Player 1 (Left)** | `W` | `S` |
| **Player 2 (Right)** | `↑` (Up Arrow) | `↓` (Down Arrow) |

**Common Controls:**
- `ESC` - Exit game
- `Enter` - Start game from menu

### Paddle Mechanics
- **Paddle Length**: 5 characters tall
- **Movement Range**: Full vertical screen (with boundaries)
- **Speed**: Controlled delay for smooth movement
- **Boundaries**: Cannot move beyond top/bottom borders

### Ball Physics System

The ball's trajectory depends on which part of the paddle it hits:

```
┌─────────────┐
│  Zone 1     │  → Ball goes UP-LEFT/UP-RIGHT (steep angle)
├─────────────┤
│  Zone 2     │  → Ball goes UP-LEFT/UP-RIGHT (medium angle)
├─────────────┤
│  Zone 3     │  → Ball goes STRAIGHT LEFT/RIGHT
├─────────────┤
│  Zone 4     │  → Ball goes DOWN-LEFT/DOWN-RIGHT (medium angle)
├─────────────┤
│  Zone 5     │  → Ball goes DOWN-LEFT/DOWN-RIGHT (steep angle)
└─────────────┘
```

### Scoring System
- **+1 Point**: Each successful paddle hit
- **Win**: When opponent misses the ball
- **Display**: Real-time score updates on screen
- **Persistence**: Final scores shown at game over

### Collision Detection
1. **Paddle Collision**: Checks all 5 zones of paddle
2. **Wall Collision**: Top and bottom boundaries reflect ball
3. **Side Collision**: Left/right edges trigger game over
4. **Boundary Checks**: Prevents paddles from leaving screen

## 💻 Installation & Compilation

### Prerequisites

**Option 1: DOSBox (Recommended for Modern Systems)**
- Download DOSBox from [dosbox.com](https://www.dosbox.com/)
- NASM assembler
- Any text editor

**Option 2: Native DOS Environment**
- MS-DOS or FreeDOS
- TASM/MASM assembler

### Compilation Instructions

#### Using NASM

```bash
# Assemble the source code
nasm pingpong.asm -o pingpong.com

# Run in DOSBox
dosbox pingpong.com
```

#### Using TASM

```bash
# Assemble
tasm /zi pingpong.asm

# Link
tlink /v /t pingpong.obj

# Run
pingpong.com
```

#### DOSBox Configuration

1. **Mount your directory**:
```
mount c: /path/to/your/game
c:
```

2. **Run the game**:
```
pingpong.com
```

### Project Files

```
pingpong-game/
├── pingpong.asm       # Main source code
├── README.md          # This file
└── LICENSE            # License information
```

## 🎮 How to Play

### Starting the Game

1. **Launch the executable** in DOSBox or DOS environment
2. **Read the welcome screen** - Press any key to continue
3. **View game menu** - Read the instructions
4. **Press Enter** to start the game

### Game Flow

```
┌──────────────────┐
│  Welcome Screen  │
└────────┬─────────┘
         │
         v
┌──────────────────┐
│   Instructions   │
└────────┬─────────┘
         │
         v
┌──────────────────┐
│   Game Screen    │
│                  │
│  [P1]  |  [P2]   │
│    🏓            │
│                  │
└────────┬─────────┘
         │
         v
┌──────────────────┐
│  Game Over       │
│  Winner: Player X │
│  Final Scores    │
└──────────────────┘
```

### Gameplay Tips

✅ **Aim for Center**: Hitting with the middle zone sends ball straight (easier to predict)  
✅ **Use Angles**: Top/bottom zones create diagonal shots (harder for opponent)  
✅ **Stay Centered**: Keep paddle in middle for better coverage  
✅ **Predict Movement**: Watch ball direction to anticipate next move  
✅ **React Quickly**: Ball speed increases gameplay intensity

## 🔧 Technical Implementation

### Memory Layout

```
Segment Map:
┌──────────────────────────────┐
│  0x0000 - 0x03FF : IVT       │  Interrupt Vector Table
├──────────────────────────────┤
│  0x0400 - 0x04FF : BDA       │  BIOS Data Area
├──────────────────────────────┤
│  0x0100 - Code Segment       │  Program Code
├──────────────────────────────┤
│  0xB800 - Video Memory       │  Text Mode Display
└──────────────────────────────┘
```

### Video Memory Structure

```
Each character cell = 2 bytes:
┌──────────┬──────────┐
│ Byte 0   │ Byte 1   │
│ ASCII    │ Attribute│
└──────────┴──────────┘

Attribute byte format:
┌─────┬─────┬─────┬─────┐
│ Blink│ BG  │ Int │ FG  │
│  1   │ 3   │ 1   │ 3   │
└─────┴─────┴─────┴─────┘

Examples:
0x07 = White on Black
0x2F = White on Green (used for ball)
0x17 = White on Blue (used for borders)
```

### Key Algorithms

#### 1. Screen Clearing
```assembly
clrscr:
    mov ax, 0xb800
    mov es, ax
    xor di, di
    mov cx, 2000        ; 80x25 = 2000 cells
    mov ax, 0x0FDB      ; Space with attribute
    rep stosw           ; Fill screen
    ret
```

#### 2. Collision Detection
```assembly
checkCollision:
    ; Compare ball position with paddle zones
    mov bx, [paddlePos]
    cmp si, bx          ; Zone 1
    je hit_zone1
    add bx, 160         ; Next line
    cmp si, bx          ; Zone 2
    je hit_zone2
    ; ... continue for all 5 zones
```

#### 3. Ball Movement
```assembly
moveBallDiagonallyDown:
    mov ax, 0x0FDB      ; Clear old position
    mov [es:si], ax
    add si, 162         ; Move down-right (160+2)
    mov ax, 0x2F00      ; Draw new position
    mov [es:si], ax
    ret
```

#### 4. Delay Loop
```assembly
delay:
    mov dword[counter], 100000
delayLoop:
    dec dword[counter]
    cmp dword[counter], 0
    jne delayLoop
    ret
```

### Subroutine Organization

| Subroutine | Purpose | Lines |
|------------|---------|-------|
| `clrscr` | Clear screen | 20 |
| `centerLine` | Draw center divider | 25 |
| `border` | Draw top/bottom borders | 35 |
| `game` | Main game loop | 50 |
| `inputP1` | Player 1 controls | 25 |
| `inputP2` | Player 2 controls | 25 |
| `movePlayer1Up/Down` | P1 movement | 40 |
| `movePlayer2Up/Down` | P2 movement | 40 |
| `moveBall*` | 6 ball movement functions | 150 |
| `checkPlayer1HitsBall` | P1 collision detection | 50 |
| `checkPlayer2HitsBall` | P2 collision detection | 50 |
| `printNum` | Display numbers | 40 |
| `gameOverr` | End game screen | 60 |
| `intro` | Welcome screen | 50 |
| `gameMenu` | Instructions | 80 |

**Total:** ~800+ lines of Assembly code

## 📸 Screenshots

### Welcome Screen
```
┌────────────────────────────────────────┐
│                                        │
│      Welcome to Ping Pong Game         │
│      -------------------------         │
│                                        │
│      Creator:                          │
│      23f-0807, 23F-0844, 23f-0839      │
│                                        │
│      Press any key to continue...      │
│                                        │
└────────────────────────────────────────┘
```

### Game Menu
```
┌──────────────────────────────────────── ┐
│  Game Menu:                             │
│                                         │
│  Player 1 -> Left   Player 2 -> Right   │
│                                         │
│  1. Pressing w will move player1 up     │
│  2. Pressing s will move player1 down   │
│  3. Pressing ↑ will move player2 up     │
│  4. Pressing ↓ will move player2 down   │
│  5. Control transfers on each hit       │
│  6. Ball touching left = Player 2 Wins  │
│  7. Ball touching right = Player 1 Wins │
│                                         │
│  Press Enter to start...                │
└────────────────────────────────────────-┘
```

### Gameplay
```
##########################################
#                            |           #
#  █                         |           #
#  █                         |           #
#  █             🏓         |           #
#  █                         |         █ #
#  █                         |         █ #
#                            |         █ #
#                            |         █ #
#                            |         █ #
##########################################
Player 1 Score: 5    Player 2 Score: 3
```

### Game Over
```
┌────────────────────────────────────────┐
│                                        │
│      Player 1 Score: 12                │
│      Player 2 Score: 10                │
│                                        │
│         Player 1 Wins!                 │
│                                        │
│         GAME OVER!!!!                  │
│                                        │
└────────────────────────────────────────┘
```

## 💡 Challenges & Solutions

### Challenge 1: Real-Time Input
**Problem**: Reading keyboard input without blocking game execution  
**Solution**: Used INT 16h, AH=01h (non-blocking check) before INT 16h, AH=00h (blocking read)

### Challenge 2: Smooth Animation
**Problem**: Ball/paddle movement too fast or flickery  
**Solution**: Implemented delay loop and clear-then-draw approach

### Challenge 3: Ball Physics
**Problem**: Complex directional movement based on hit position  
**Solution**: 5-zone paddle detection with separate movement subroutines for each trajectory

### Challenge 4: Score Display
**Problem**: Converting binary numbers to ASCII for display  
**Solution**: Implemented `printNum` with division by 10 and ASCII conversion

### Challenge 5: Collision Edge Cases
**Problem**: Ball could get stuck or pass through paddles  
**Solution**: Precise position checking with 160-byte line offset calculations

### Challenge 6: Memory Management
**Problem**: Limited stack space for local variables  
**Solution**: Efficient use of BP-relative addressing with minimal locals

## 🚀 Future Enhancements

### Gameplay Improvements
- [ ] **AI Opponent**: Single-player mode with computer AI
- [ ] **Difficulty Levels**: Variable ball speed (Easy, Medium, Hard)
- [ ] **Ball Speed Increase**: Progressive speed up during rally
- [ ] **Power-Ups**: Special abilities (speed boost, larger paddle, slow-mo)
- [ ] **Multiple Balls**: Chaos mode with 2-3 balls simultaneously
- [ ] **Tournament Mode**: Best of 3/5 matches

### Technical Enhancements
- [ ] **Sound Effects**: PC speaker beeps for hits/scores
- [ ] **VGA Graphics**: Upgrade to 320x200 graphics mode
- [ ] **Smoother Animation**: Implement double buffering
- [ ] **Replay System**: Save and replay best matches
- [ ] **High Score Table**: Persistent leaderboard with file I/O
- [ ] **Network Play**: Serial port multiplayer

### Visual Improvements
- [ ] **Better Graphics**: ASCII art paddles and ball trail
- [ ] **Color Themes**: Different color schemes
- [ ] **Particle Effects**: Ball trail or hit effects
- [ ] **Menu System**: Proper game menu with selections
- [ ] **Pause Function**: Freeze game mid-play

## 🤝 Contributing

Contributions are welcome! Whether you're fixing bugs, adding features, or improving documentation.



## 📄 License

-This project is created for educational purposes as part of a university course assignment.

## 👥 Authors

**Team Members:**
- **23f-0807** - [https://github.com/ayesha189]

## 🙏 Acknowledgments

- Classic Pong game by Atari for inspiration
- Assembly Language course materials and resources
- DOSBox team for the excellent DOS emulator
- NASM community for documentation and support
- Ralf Brown's Interrupt List for BIOS interrupt reference
- Our instructor for guidance and feedback

---

## 📝 Academic Details

**Course**: Assembly Language Programming / Computer Organization  
**Project Type**: Final Project  


### Learning Outcomes Achieved
✅ x86 Assembly Language syntax and structure  
✅ BIOS interrupt services (INT 10h, 16h, 21h)  
✅ Direct video memory manipulation  
✅ Stack operations and subroutine calls  
✅ Real-time input handling  
✅ Addressing modes and memory management  
✅ Conditional jumps and program flow control  
✅ String operations and repeat instructions  

---

## 🎓 Technical Specifications

- **Assembly Dialect**: NASM (Netwide Assembler)
- **Instruction Set**: x86 (16-bit)
- **Memory Model**: Tiny (.COM executable)
- **Video Mode**: Text Mode 3 (80x25, 16 colors)
- **Execution Environment**: DOS Real Mode
- **Code Size**: ~800 lines
- **Executable Size**: < 1 KB


## 📚 Resources

### Assembly Language Learning
- [x86 Assembly Guide](https://www.cs.virginia.edu/~evans/cs216/guides/x86.html)
- [NASM Documentation](https://www.nasm.us/docs.php)
- [Ralf Brown's Interrupt List](http://www.ctyme.com/rbrown.htm)

### DOS Programming
- [DOSBox Documentation](https://www.dosbox.com/wiki/)
- [BIOS Interrupt Reference](http://www.ctyme.com/intr/int.htm)
- [PC Game Programmer's Encyclopedia](http://bespin.org/~qz/pc-gpe/)

### Game Development
- [Classic Game Postmortem](https://www.youtube.com/watch?v=Pm-9X4FWPVw) - Pong History
- [Retro Game Mechanics Explained](https://www.youtube.com/@RetroGameMechanicsExplained)

---

⭐ **If you enjoyed this retro gaming experience or learned from the code, please give it a star!**

🏓 **Game On!** 🎮

---
