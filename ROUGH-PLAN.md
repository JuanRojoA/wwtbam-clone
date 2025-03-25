## I. Environment & Project Setup

1. **General Project Configuration:**

   - Verify that the Vite + React project is correctly scaffolded.
   - Set up BiomeJS, and Git hooks (optional but best practice).
   - Create and document a README file that outlines project setup and environment notes.

2. **Environment Variables & Configuration:**

   - Create a `.env` file for Vite with Supabase API keys and endpoints.
   - Add any additional configuration (e.g., URL endpoints for assets/sounds).
   - Configure Vite to expose the environment variables securely.

3. **TailwindCSS Setup:**
   - Verify the TailwindCSS installation.
   - Adjust the `index.css` to include themes, custom colors, or breakpoints if needed.
   - Set up basic global styling and mobile responsiveness defaults.

---

## II. Supabase Integration & Database Schema

1. **Supabase Project Initialization:**

   - Create a new Supabase project.
   - Obtain the API keys and configure the client in your React project.

2. **Database Schema Design:**

   - **Table: `games`**
     - Columns:
       - `id` (primary key)
       - `title` (French text, e.g., "Qui Veut Gagner des Millions – Edition 1")
       - `description` (optional)
       - `created_at` (timestamp)
   - **Table: `questions`**
     - Columns:
       - `id` (primary key)
       - `game_id` (foreign key referencing `games.id`)
       - `question_text` (French text)
       - `question_order` (values 1 to 4)
   - **Table: `answers`**
     - Columns:
       - `id` (primary key)
       - `question_id` (foreign key referencing `questions.id`)
       - `answer_text` (French text)
       - `is_correct` (boolean flag)
   - _(Optionally, if you want to extend functionality later, consider a `scores` or `users` table, but note that no leaderboard is required at this time.)_

3. **API Development & Data Fetching:**
   - Write helper methods to query games, questions, and answers from Supabase.
   - Test queries locally to ensure the structure works as expected.
   - _(If needed later, add simple authentication for admin CRUD operations.)_

---

## III. Routing & Navigation (React Router)

1. **Define Main Routes:**

   - **Home/Selection Page:**
     - Route: `/`
     - Displays a list/grid of available games (at least six).
   - **Game Session Page:**
     - Route: `/game/:gameId`
     - Loads the selected game’s information along with its questions and answers.
   - **Fallback/NotFound Page:**
     - For any undefined routes.

2. **Implement Navigation:**
   - Use `<Link>` components for navigation between pages.
   - Ensure that the game selection screen links to the game session page with the proper game ID.

---

## IV. UI Components & Layout

1. **Page Layout Components:**

   - **Header/Footer (Optional):**
     - For branding (use French titles and texts).
   - **Responsive Grid/Layout:**
     - Ensure a responsive layout using TailwindCSS grid or flex utilities.

2. **Home/Game Selection Page:**

   - **Game Card Component:**
     - Displays the game “title,” a short description, and a “Start” button.
     - Style the card with TailwindCSS ensuring mobile-friendly sizing.
   - **List/Grid Layout:**
     - Render at least six game cards.
     - Implement click navigation using React Router.

3. **Game Session Page:**

   - **Game Container:**
     - Manage overall game state (current question, status, score, used wildcards).
     - Trigger data fetching (using the gameId from the URL) to load the questions and corresponding answers from Supabase.
   - **Question Display Component:**
     - Shows the current question text (in French).
     - Apply a clear visual hierarchy (large text, centered).
   - **Answer Options Component:**
     - For each question, render four buttons representing the answer options.
     - Each button should:
       - Display the answer text.
       - Handle onClick events to check correctness.
       - Play a selection sound upon click.
       - Change appearance based on selection (e.g., hover effects, active state).
   - **Wildcards (Jokers) Component:**
     - Two separate buttons:
       1. **50/50 Wildcard:**
          - On click, run a function to remove two incorrect answer buttons (keep the correct one and one randomly selected wrong answer).
          - Visually mark the wildcard as used (disable it or overlay with an “X”/gray out).
       2. **Call a Friend Wildcard:**
          - On click, do nothing functionality-wise—but mark it as used when clicked.
          - Optionally, show a tooltip like “Ce joker n’est pas fonctionnel”.
   - **Feedback/Modal Components:**
     - **Correct Answer Feedback:**
       - Show a message “Correct !” and play the triumphant music.
       - Use a transition/animation (e.g., fade in/out) before moving to the next question.
     - **Game Over Screen:**
       - When an incorrect answer is clicked, display a game-over message (e.g., “Mauvaise réponse. Vous avez perdu !”) along with the disappointing sound.
       - Provide a button to return to the Home/Selection page.
     - **Victory Screen:**
       - After answering all four questions correctly, display a winning message (e.g., “Félicitations, vous avez gagné !”).
       - Play celebratory, longer triumphant music.
       - Provide a restart link or navigation back to the selection page.

4. **Audio Management:**

   - Create an **Audio Manager** (or custom hook, e.g., `useSound`) to:
     - Play the selection sound whenever an answer is clicked.
     - Play the correct answer sound/music.
     - Play the game-over sound/music upon incorrect answer.
     - Play the celebratory winning music at game completion.
   - Preload audio assets and adjust volume as needed.

5. **Animations & Transitions:**
   - Use TailwindCSS classes or CSS transitions/animations when:
     - Transitioning between questions.
     - Disabling wildcards.
     - Showing modals (correct answer, game over, victory).
   - Ensure the transitions are smooth and enhance the user experience.

---

## V. Game Flow Logic & State Management

1. **State Management Setup:**

   - Choose between React’s built-in state (`useState`, `useReducer`) or set up a context (e.g., `GameContext`) to hold:
     - Current game information (fetched from Supabase).
     - Current question index.
     - Game status flags: “in progress,” “game over,” “won.”
     - Wildcard usage states (e.g., `fiftyUsed`, `callAFriendUsed`).
     - Display state for answer options (especially after 50/50 is utilized).

2. **Event Handling:**

   - **Answer Selection Handler:**
     - Check if the selected answer is correct:
       - **If Correct:**
         - Play the "correct" audio.
         - Show a “Correct !” message.
         - If it’s the final (4th) question, transition to the Victory Screen.
         - Otherwise, set a small delay before loading the next question.
       - **If Incorrect:**
         - Immediately show the Game Over Screen.
         - Play the "disappointing" audio.
   - **50/50 Wildcard Handler:**
     - On click:
       - Randomly remove/hide two incorrect answer options.
       - Ensure one incorrect option remains alongside the correct answer.
       - Update the wildcard state to disable further use.
   - **Call a Friend Wildcard Handler:**
     - On click:
       - Mark the wildcard as used (disable further clicks).
       - No other functionality is performed.

3. **Game Progression Flow:**
   - Load the first question on game start.
   - After each answer selection, update the question index accordingly.
   - Handle mid-game transitions (e.g., animations, delays in processing to allow audio playback).
   - Handle final state transitions (game over or victory).

---

## VI. Styling & Mobile Responsiveness

1. **TailwindCSS-Based Styling:**

   - Develop a consistent design system using TailwindCSS utility classes.
   - Use responsive utilities (e.g., `sm:`, `md:`, `lg:`) to adjust layouts for mobile vs. desktop.
   - Create custom component classes where necessary for consistency.

2. **Mobile & Touch Optimization:**

   - Ensure clickable areas (answer buttons, wildcards) are large enough.
   - Test the layout on various devices/screen sizes.
   - Optimize fonts and spacing for readability.

3. **Animations:**
   - Use TailwindCSS transitions for button hover and pressed states.
   - Add subtle animations (e.g., fade-ins for modals, scale transitions for buttons) to enhance interactivity.

---

## VII. Testing, Quality Assurance & Best Practices

1. **Functionality Testing:**

   - Manually test the entire game flow:
     - Question loading via Supabase.
     - Correct vs. incorrect answer handling.
     - Wildcard functionality (50/50 and Call a Friend).
     - Audio playback for all events.
   - Verify that each game session correctly uses one of six independent sets of questions.

2. **Responsive Testing:**

   - Test the UI on various screen sizes (mobile, tablet, desktop).
   - Validate that the responsive behavior does not interfere with game interaction.

3. **Code Quality & Documentation:**

   - Comment key pieces of game logic.
   - Document the state management approach.
   - Ensure meaningful commit messages and version control practices.
   - Write unit or integration tests if possible (e.g., using Jest/React Testing Library).

4. **Localization:**
   - Verify that all UI text, prompts, and messages are in French.
   - Confirm that any hard-coded texts or audio cues match the “Qui Veut Gagner des Millions” theme.

---

## VIII. Deployment & Final Steps

1. **Production Build:**

   - Configure Vite for production builds.
   - Verify that all environment variables (for Supabase, audio assets, etc.) are correctly set.

2. **Supabase Production:**

   - Ensure the Supabase project is set up with the proper production database.
   - Secure your API keys and verify RLS (Row-Level Security) if needed.

3. **Final Testing & Deployment:**

   - Run penetration, performance, and usability tests.
   - Deploy the app to your chosen hosting provider (e.g., Vercel, Netlify).
   - Monitor the production environment for any issues.

4. **Post-Deployment:**
   - Document any configuration steps required for future maintenance.
   - Set up a feedback mechanism for bug reporting or future enhancements.
