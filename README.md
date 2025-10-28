# ResolveHub — Ticket resolution that actually moves work forward

ResolveHub is a focused ticket management app built with Vue 3 and Vite. It helps teams capture issues, assign owners, prioritize work, and close tickets quickly — without unnecessary process overhead. The UI and features are designed for day-to-day ticket operations: triage, assignment, progress tracking, and resolution.

Core features

- Fast ticket creation and editing (full CRUD)
- Simple status workflow: Open → In Progress → Closed
- Priority and tagging to drive work focus
- Compact dashboard with actionable stats
- Lightweight modals and toast notifications for fast feedback
- Client-side persistence (LocalStorage) for quick demos and trials
- Responsive design for desktop and tablet use

## Prerequisites

- Node.js (v14 or higher)
- npm or yarn

## Installation & local run

Clone the repo, install deps and run the dev server:

```bash
cd ticketflow-vue
npm install
npm run dev
```

Vite will print the local URL (for example http://localhost:5175). Open that URL in your browser to view ResolveHub.

## Build & preview

Create an optimized build and preview it locally:

```bash
npm run build
npm run preview
```

The `dist` folder contains the production build.

## Demo credentials

- Email: demo@resolvehub.com
- Password: demo123

(Or create a new account from the signup form.)

## Project Structure
```
src/
├── App.vue                     # Main application router
├── main.js                     # Vue entry point
└── components/
    ├── LandingPage.vue         # Home page
    ├── AuthPage.vue            # Login/Signup
    ├── Dashboard.vue           # Dashboard with stats
    ├── TicketManagement.vue    # CRUD operations
    ├── Toast.vue               # Toast notifications
    └── Modal.vue               # Modal dialogs
```

## Features Breakdown


## App areas — what matters for users

Landing page — direct summary of capability and CTA to sign up or sign in.

Authentication — simple local demo accounts with validation and session persistence for quick trials.

Dashboard — at-a-glance ticket metrics and quick access to the ticket board so teams can see where to focus.

Ticket board — create tickets, set priority and status, assign ownership (via notes), edit details, and delete when work is complete. Designed to minimize clicks and keep the team focused on resolution.

## Technologies

- Vue 3
- Vite
- Scoped CSS
- LocalStorage (demo persistence)

## Technologies Used

- Vue.js 3 (Composition API)
- Vite (Build tool)
- Scoped CSS
- LocalStorage for data persistence
- Responsive design with CSS Grid and Flexbox

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## Deployment

### Netlify
1. Run `npm run build`
2. Drag the `dist` folder to Netlify Drop

### Vercel
```bash
npm install -g vercel
vercel
```

### GitHub Pages
Install gh-pages:
```bash
npm install --save-dev gh-pages
```

Add to package.json scripts:
```json
"deploy": "npm run build && gh-pages -d dist"
```

Deploy:
```bash
npm run deploy
```

## License

MIT License

## Author

Victor O. — Frontend Wizards Cohort