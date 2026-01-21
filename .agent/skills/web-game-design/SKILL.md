---
name: web-game-design
description: Next.js + Game Engine (Phaser, ThreeJS) bridge architecture and modern web game design.
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Web Gaming Design (Next.js Wrapper)

This skill teaches how to wrap and integrate web games with modern web technologies (specifically Next.js).

## 1. Next.js Wrapper Architecture

The game engine should run as a sub-layer inside Next.js.

### Container Structure
```tsx
// components/GameWrapper.tsx
'use client';
import dynamic from 'next/dynamic';
import { useRef, useEffect } from 'react';

const GameComponent = dynamic(() => import('./PhaserGame'), { ssr: false });

export default function GameWrapper() {
  const gameRef = useRef(null);

  return (
    <div className="relative w-full h-screen overflow-hidden">
      <GameComponent ref={gameRef} />
      <HUDLayer gameRef={gameRef} />
    </div>
  );
}
```

## 2. React-Engine Bridge

### Data Flow (React -> Engine)
Access the game instance via a `ref` and call its methods or emit events.

```tsx
const handleJump = () => {
  gameRef.current.events.emit('PLAYER_JUMP');
};
```

### Data Flow (Engine -> React)
Emit a Custom Event from within the game and update state by listening to it inside React `useEffect`.

```tsx
// Inside Game code
this.events.emit('UPDATE_SCORE', 100);

// Inside React useEffect
gameInstance.on('UPDATE_SCORE', (score) => setScore(score));
```

## 3. Hybrid UI (DOM + Canvas)

- **Canvas**: Should only be used for the game world, characters, and animations.
- **DOM (React + Tailwind)**: Should be used for Menus, Inventory, Leaderboards, and Modal Dialogs.
- **Benefits**: SEO-friendly text, easy styling (CSS), rapid development, and accessibility.

## 4. Multiplayer Integration with Phoenix Channels

Provide real-time multiplayer experience by connecting the game state to Phoenix Channels via Next.js.

1. **Connect**: Connect to channel when Next.js component mounts.
2. **Sync**: Forward every incoming game packet (tick) to the game engine.
3. **Optimistic Updates**: Perform prediction on client side to prevent input lag.

---

> **Principle:** The game library (Phaser/Threejs) may change, but the Next.js wrapper architecture forms the foundation of the project.
