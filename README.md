# Who Wants to Be a Millionaire - Clone

A modern clone of the famous TV game show "Who Wants to Be a Millionaire" (WWTBAM) built with React, TypeScript, Vite, and Supabase.

## Features

- Six different game sessions with unique sets of questions
- Authentic game mechanics including the iconic 50/50 and Call-a-Friend wildcards
- Sound effects and music for immersive gameplay experience
- Fully responsive design for mobile, tablet, and desktop
- French language interface (Qui Veut Gagner des Millions)

## Technology Stack

- **Frontend**: React 19 with TypeScript
- **Build Tool**: Vite 6
- **Styling**: TailwindCSS 4
- **Routing**: React Router 7
- **Database**: Supabase
- **Code Quality**: BiomeJS

## Getting Started

### Prerequisites

- Node.js (v18.0.0 or higher)
- Bun package manager (recommended)

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/yourusername/wwtbam-clone.git
   cd wwtbam-clone
   ```

2. Install dependencies:

   ```bash
   bun install
   ```

3. Set up environment variables:

   ```bash
   cp .env.example .env
   ```

   Update the `.env` file with your Supabase credentials.

4. Start the development server:

   ```bash
   bun run dev
   ```

5. Open [http://localhost:5173](http://localhost:5173) in your browser.

## Project Structure

```
wwtbam-clone/
├── src/
│   ├── assets/       # Audio files, images, etc.
│   ├── components/   # Reusable UI components
│   ├── lib/          # Supabase client and API utilities
│   └── utils/        # Helper functions and custom hooks
├── public/           # Static assets
└── ...configuration files
```

## Game Flow

1. Select one of six available game sessions from the home screen
2. Answer a series of increasingly difficult questions
3. Use wildcards strategically when needed
4. Win the game by correctly answering all questions

## Development

- Run development server: `bun run dev`
- Build for production: `bun run build`
- Preview production build: `bun run preview`

## License

This project is for educational purposes only. The "Who Wants to Be a Millionaire" format and branding are the property of their respective owners.
