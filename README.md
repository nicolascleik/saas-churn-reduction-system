# EngPlay - Cancellation and Retention Module (Churn Reduction)

This project consists of the Front-end implementation and Back-end integration (Firebase) of the subscription cancellation flow for the **EngPlay** course platform.

Developed by **Squad 28** as part of the Software Residency delivery for Porto Digital / UNIT.

## Problem & Solution

**The Challenge:** Subscription services face high cancellation rates (*churn*), often without understanding the real reasons for user departure or having the chance to reverse the decision.

**Our Solution:** We developed an intelligent flow that not only processes the cancellation but acts as a strategic retention tool.
* **Diagnosis:** Collection of qualitative data (written feedback) and quantitative data (categorized reasons).
* **Soft Landing (72h):** Cancellation is not immediate. The system schedules the termination for 72 hours later, keeping access active. This allows the CS (Customer Success) team to use the collected data to contact the user and attempt recovery strategies (*Win-back*) before the definitive block.

---

## The Team (Squad 28)

Collaboratively developed by:

* [@nicolascleik](https://github.com/nicolascleik)
* [@ThiiagoAlvess](https://github.com/ThiiagoAlvess)
* [@joserenato06](https://github.com/joserenato06)
* [@Gustavoac0909](https://github.com/Gustavoac0909)
* [@FernandaSampaio15](https://github.com/FernandaSampaio15)
* [@luisfcosta-byte](https://github.com/luisfcosta-byte)
* [@CaioSouza0121](https://github.com/CaioSouza0121)
* [@Erick2029-del](https://github.com/Erick2029-del)

---

## Key Features

### 1. Retention & Diagnosis Flow
[cite_start]Based on real user personas (Rafael, Juliana, Beatriz, Lucas, and Carla) [cite: 11-208], the flow guides the user through psychological stages:
* **Persuasive Journey:** Impact stages (loss aversion regarding progress/certificates) before the final decision.
* **Qualitative Feedback:** Star rating (1-5) for Content, Didactics, and Usability.
* **Quantitative Feedback:** Selection of reasons (e.g., "Financial", "Lack of Time", "Content").

### 2. Accessibility & Inclusion (Highlight)
The project was built with a strong focus on accessibility:
* **Read Mode (Text-to-Speech):** Implementation of the native **Web Speech API**. Users can activate a mode where the site reads texts and buttons aloud upon clicking, facilitating use for people with low vision or dyslexia.
    * *Safety:* Accidental clicks are blocked while the text is being read.
* **VLibras:** Floating widget for Brazilian Sign Language (Libras) translation integrated into all screens.

### 3. Personalization & UI
* **Dark/Light Theme:** Theme switching system with persistence via `localStorage` (the site "remembers" the user's choice).
* **Responsive Sidebar:** Retractable side menu that adapts to mobile and desktop, also preserving state.

### 4. Security & Compliance
* **Smart Lock:** The "Confirm Cancellation" button starts as **disabled**.
* **Legal Validation:** The user is forced to check *"I agree to the Terms"* and *"I am aware of the Loss"* to unlock the action, ensuring legal backing against future claims.

---

## Tech Stack

Following the project's technical specification:

* **Front-End:** Semantic HTML5, CSS3 (Grid/Flexbox, Variables), JavaScript (Vanilla ES6+ Modules).
* **Accessibility:** Web Speech API (Native), VLibras Widget.
* **Back-End (BaaS):** Firebase Firestore (NoSQL Database).
* **Version Control:** Git/GitHub.

---

## Project Structure

The project separates responsibilities for styling, logic, and structure:

```text
RESIDENCIA.../
├── 📂 assets/                # Images and icons
├── 📂 CSS/
│   ├── 📂 estilo_cada_pagina/ # Specific CSS for each flow step
│   ├── 📂 estilo_layout/      # Themes (light_mode.css)
│   ├── css_geral.css          # Global resets
│   └── layout.css             # Header and Sidebar
├── 📂 HTML/
│   ├── index.html             # (1) Decision/Home
│   ├── impacto.html           # (2) Loss Awareness
│   ├── avaliacao_do_curso.html# (3) Qualitative Collection
│   ├── diagnostico.html       # (4) Reasons and DB Submission
│   ├── confirmacao.html       # (5) Termination (Soft Landing)
│   └── ...
├── 📂 scripts/
│   ├── avaliacao_do_curso.js  # Star rating logic and temporary state
│   ├── diagnostico.js         # Firebase Integration (addDoc)
│   └── script.js              # Global logic (Theme, Sidebar, Accessibility)
└── README.md

```

---

## How to Run

⚠️ **Note:** Since the project uses **ES6 Modules** to connect to Firebase, you need a local HTTP server.

1. **Clone the repository:**
```bash
git clone [https://github.com/nicolascleik/Residencia-de-Software-I---UNIT.git](https://github.com/nicolascleik/Residencia-de-Software-I---UNIT.git)

```


2. **Open in VS Code:**
Open the root project folder.
3. **Use Live Server:**
* Install the **Live Server** extension in VS Code.
* Right-click on `HTML/index.html`.
* Select **"Open with Live Server"**.


4. **Test:** The browser will open the flow. Follow the steps to confirmation to generate a database record.

---

## Database (Firestore)

The solution uses two-layer persistence:

1. **Local (Client-Side):** `localStorage` to keep the theme and evaluation ratings while the user navigates between HTML pages.
2. **Cloud (Server-Side):** Upon confirmation, a unified JSON document is sent to the `cancelamentos` collection in Firestore.

### JSON Document Schema:

*Note: The keys are stored in Portuguese to match the backend implementation.*

```json
{
  "aluno": {
    "nome": "João da Silva",
    "email": "joao.aluno@email.com"
  },
  "feedback": {
    "estrelas": [
       {"categoria": "Conteúdo", "nota": 5},
       {"categoria": "Didática", "nota": 4},
       {"categoria": "Usabilidade", "nota": 2}
    ],
    "comentario": "Gostei do conteúdo, mas o app trava muito."
  },
  "motivosCancelamento": ["Usabilidade / Bugs", "Financeiro"],
  "metadata": {
    "canceladoEm": "Timestamp (ISO 8601)",
    "displayData": "22/11/2023, 10:30:00"
  }
}

```

---

## Workflow Logic

The technical flow follows this pipeline :

1. **Initialization:** `script.js` checks user preferences (Theme/Menu) on load to avoid FOUC (Flash of Unstyled Content).
2. **Temporary State:** In `avaliacao_do_curso.html`, ratings are saved in the browser (`localStorage`) so they aren't lost during page transition.
3. **Consolidation:** In `diagnostico.html`, the script retrieves ratings from storage, validates security checkboxes, and sends the complete package to Firebase.
4. **Soft Landing:** After success (HTTP 200), the user sees the `confirmacao.html` screen with the 72-hour access notice.

---

**Developed for the Software Residency I course - 2025.**

```

```
