# TicketFlow - Vue.js Version

A modern, responsive ticket management system built with Vue.js 3.

## Features

✅ Landing page with wavy SVG background and decorative circles
✅ Authentication (Login/Signup) with validation
✅ Protected dashboard with ticket statistics
✅ Full CRUD ticket management
✅ Toast notifications
✅ Modal dialogs
✅ Form validation
✅ LocalStorage persistence
✅ Responsive design (max-width: 1440px)
✅ Status colors: Green (open), Amber (in progress), Gray (closed)

## Prerequisites

- Node.js (v14 or higher)
- npm or yarn

## Installation

1. Clone/download this folder
2. Navigate to the project directory:
```bash
   cd ticketflow-vue
```
3. Install dependencies:
```bash
   npm install
```

## Running the App

Start the development server:
```bash
npm run dev
```

The app will open at [http://localhost:3000](http://localhost:3000)

## Building for Production

Create an optimized production build:
```bash
npm run build
```

Preview the production build:
```bash
npm run preview
```

The `dist` folder will contain the production-ready files.

## Demo Credentials

- **Email:** demo@ticketflow.com
- **Password:** demo123

Or create a new account via the signup page.

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

### Landing Page
- Hero section with wavy SVG
- Decorative circles
- Feature cards
- CTA sections
- Footer

### Authentication
- Login/Signup forms
- Email and password validation
- Toast notifications
- Session management via localStorage

### Dashboard
- Total tickets stat
- Open tickets stat
- In Progress stat
- Resolved tickets stat
- Quick action button

### Ticket Management (CRUD)
- Create new tickets
- View all tickets in card layout
- Edit existing tickets
- Delete with confirmation
- Real-time validation
- Status: open, in_progress, closed
- Priority: low, medium, high

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

MIT License - Built for Frontend Wizards Stage 2

## Author

Victor O. - Frontend Wizards Cohort