# ERASURE

> A story that reads you back — and never lets go.

ERASURE is an experimental narrative web experience that combines:

- Real‑time 3D Dreamware characters (Three.js + GLTF)
- AI‑driven stories (Collective Dream Engine)
- Mood‑based branching (mystical, dark, hopeful, surreal, horror, random)
- Optional webcam input (for presence / emotion)

The app is built with **Vite**, **Three.js**, **TensorFlow.js**, and a custom Node/Socket.io backend.

---

## ✨ Features

- **3D Dreamware Companions**
  - Up to three separate GLB characters (`main.glb`, `main2.glb`, `main3.glb`)
  - Idle motion (bobbing, swaying, slow rotation)
  - Entrance / exit animations
  - Mood‑dependent behavior (happy / sad / angry / reading)
  - Optional GLB swap (e.g. `dying.glb` for specific moods)

- **Adaptive Story Engine**
  - Moods: `mystical`, `dark`, `hopeful`, `surreal`, `horror`, `random`
  - Online stories from the backend (“ERASURE’s consciousness”)
  - Local fallback stories if online fetch fails
  - Typewriter effect + mood‑based continuation prompts

- **Voice & Interaction**
  - Text‑to‑speech narrator that:
    - Reads the **entire** story after you click **Proceed**
    - Asks: “How are you feeling right now inside this story?” mid‑way
  - Optional webcam (graceful fallback if denied)
  - Mood selector UI + landing mood spheres on the 3D globe

- **Backend & Collective Layer**
  - Node.js + Socket.io server at `http://localhost:3000`
  - Tracks active “readers”
  - Pluggable engines for text, emotions, temporal predictions, export

---

## 🧱 Tech Stack

- **Frontend**
  - Vite (bundler/dev server)
  - Three.js (3D rendering)
  - GLTFLoader (GLB loading)
  - TensorFlow.js + COCO‑SSD
  - Tone.js (audio engine)
  - GSAP (animation)
  - Socket.io client, Compromise (NLP), jsPDF, html2canvas

- **Backend**
  - Node.js
  - Express / HTTP server
  - Socket.io server

---

## 📦 Project Structure

```text
erasure/
├─ public/
│  ├─ models/
│  │  └─ character/
│  │     ├─ main.glb
│  │     ├─ main2.glb
│  │     ├─ main3.glb
│  │     ├─ dying.glb
│  │     ├─ happy.glb       # optional animation-only clips
│  │     ├─ sad.glb
│  │     ├─ talk.glb
│  │     └─ angry.glb
│  └─ ...
├─ src/
│  ├─ app.js                 # main app orchestration
│  ├─ character/
│  │  └─ Character.js        # Dreamware character class
│  ├─ core/                  # QuantumTextEngine, EmotionEngine, etc.
│  └─ ...
├─ server.js                 # Node/Socket.io backend
├─ index.html
├─ public/styles.css
└─ package.json
```

---

## 🚀 Running Locally

> Requirements: **Node.js 18+** and **npm**

1. **Install dependencies**

   ```bash
   npm install
   ```

2. **Start dev servers** (backend + frontend)

   ```bash
   npm run dev
   ```

   This runs:

   - `node server.js` → `http://localhost:3000`
   - `vite` → `http://localhost:5173`

3. **Open the app**

   - Visit `http://localhost:5173/` in your browser.

---

## 🧪 Main Flow

1. **Arrival**
   - 3D scene loads (particles, globe, quantum core, mood orbits).
   - Up to three Dreamware characters appear in front of the scene.

2. **Begin Reading**
   - Click **“Begin Reading”**.
   - Browser may ask for webcam permission (optional).
   - Characters perform an entrance; primary character asks:
     - “May I see you? I would like to read with you.”

3. **Mood Selection**
   - Choose from:
     - Mystical • Dark • Hopeful • Surreal • Horror • Random
   - Sets story type and updates 3D mood layer and characters.

4. **Story Loading**
   - Tries online story for that mood.
   - Falls back to local story templates if needed.
   - Text appears via typewriter effect.

5. **Proceed & Reading**
   - Click **Proceed**:
     - Primary character reads the full story.
     - Around the middle, asks how you’re feeling inside the story.
     - At the end, characters return to neutral.

---

## 🎭 Dreamware Characters

- **Creation** (in `app.js`):

  ```js
  const positions = [
    new THREE.Vector3(-220, -140, 260),
    new THREE.Vector3(0, -140, 260),
    new THREE.Vector3(220, -140, 260)
  ];

  dreamCharacters = positions.map(
    (pos) => new Character({ scene, camera, initialPosition: pos })
  );

  const modelPaths = [
    '/models/character/main.glb',
    '/models/character/main2.glb',
    '/models/character/main3.glb'
  ];

  await Promise.all(
    dreamCharacters.map((char, idx) =>
      char.loadMainModel(modelPaths[idx] || modelPaths[0])
    )
  );
  ```

- **Idle Motion**
  - Stronger bobbing and side‑to‑side sway.
  - Slow rotation when not speaking.

- **Moods**

  `setMood(mood)` maps simple mood labels (`happy`, `sad`, `angry`, `neutral`, `reading`) to:

  - Optional animation clips (`Happy`, `Sad`, `Angry`, `Talk`)
  - Small pose tweaks (e.g., nodding down for sad)

- **Model Swap Example**

  When a specific mood is chosen, the primary character can switch GLB:

  ```js
  primary.swapMainModel('/models/character/dying.glb');
  ```

---

## ⚙️ npm Scripts

- `npm run dev` – run backend + Vite dev server
- `npm run build` – build the frontend
- `npm run preview` – preview the built frontend locally

---

## 🌐 Deployment Notes

- Frontend: Vite build output can be deployed to any static host.
- Backend: `server.js` (Node + Socket.io) should be deployed separately (or together with the frontend on a Node host).
- Update any hard‑coded backend URLs (currently `http://localhost:3000`) before deploying to production.

---

## 🔐 Permissions

- **Webcam**: optional; if denied, the app still works (no emotion detection).
- **Audio**: uses browser `speechSynthesis` + Tone.js; requires a user click to start in most browsers.

# erase
# public
