
# AI-Powered Learning Management System (AI LMS)

This project is an AI-powered Learning Management System (AI LMS) built using Next.js, Clerk for authentication, Inngest for function orchestration, Drizzle ORM for database interaction, and Google Gemini for AI-powered content generation. It allows users to create personalized study materials for various learning goals, such as exam preparation, job interviews, or general practice.

## Demo
- [Demo Video](https://drive.google.com/file/d/1CuD56rPsAf80mXNPXnVTLV0EYdY53B3z/view?t=2)


## Features

* **AI-Driven Course Outline Generation:** Users input a topic, course type (e.g., Exam, Job Interview, Practice, Code Prep, Other), and difficulty level.  The AI generates a course outline containing a summary, chapters, chapter summaries, topic lists for each chapter, and relevant emojis, all formatted in JSON.
* **Dynamic Study Material Generation:**  Based on the course outline, the system dynamically generates various types of study materials:
    * **Chapter Notes:** Detailed notes for each chapter, broken down by topics, in markdown format for easy rendering.
    * **Flashcards:** Interactive flashcards for memorization, with front and back content.
    * **Quizzes:** Gamified quizzes with multiple-choice questions, timers, and scoring to test knowledge.
    * **Question & Answer (Q&A):**  Comprehensive question-and-answer pairs for deeper learning.
* **Personalized Learning Paths:** Users can select the specific study material types they want to generate and use.  An "ALL" option generates all available study types.
* **User Authentication:** Clerk manages user registration, login, and secure sessions.
* **Database Integration:** Drizzle ORM facilitates interactions with a Neon Serverless PostgreSQL database, storing course data, user information, and generated study materials.
* **Background Task Orchestration:**  Inngest orchestrates the complex interactions between the AI model, database operations, and asynchronous content generation, providing a seamless user experience.
* **Progress Tracking (Generating/Ready Status):** UI elements indicate the status of material generation, providing feedback to the user while content is being created in the background.
* **Interactive Flashcard UI:** Flashcards are presented in a user-friendly swipeable interface with flip card animations.
* **Gamified Quiz Experience:** Quizzes include timers, scoring, and feedback on correct/incorrect answers, enhancing user engagement.
* **Markdown Rendering for Notes and Q&A:**  Notes and Q&A content are rendered using `react-markdown` and `remark-gfm` for a clean and formatted display.


## Tech Stack

* **Frontend:** Next.js (App Router), React, Tailwind CSS, Ripple UI, Lucide React, Swiper.js, React Card Flip, Styled Components
* **Backend/Serverless:**  Node.js, Inngest, Clerk
* **Database:** Neon Serverless PostgreSQL, Drizzle ORM
* **AI:** Google Gemini


## Installation and Setup

1. **Clone the Repository:**

```bash
git clone https://github.com/your-username/ai-lms.git
```

2. **Install Dependencies:**

```bash
cd ai-lms
npm install
```

3. **Environment Variables:**

Create a `.env.local` file in the project root and add the following:

```
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=YOUR_CLERK_PUBLISHABLE_KEY
CLERK_SECRET_KEY=YOUR_CLERK_SECRET_KEY
NEXT_PUBLIC_DATABASE_CONNECTION_STRING=YOUR_NEON_DATABASE_CONNECTION_STRING
NEXT_PUBLIC_GEMINI_API_KEY=YOUR_GOOGLE_GEMINI_API_KEY
```

* **`NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY`**: Your Clerk publishable key.
* **`CLERK_SECRET_KEY`**: Your Clerk secret key.
* **`NEXT_PUBLIC_DATABASE_CONNECTION_STRING`**: Your Neon database connection string.
* **`NEXT_PUBLIC_GEMINI_API_KEY`**: Your Google Gemini API key.

4. **Run the Development Server:**

```bash
npm run dev
```


## Project Structure

* **`app`**:  Next.js App Router directory containing all application code:
    * **`api`**:  API routes for backend logic.
    * **`components`**: Reusable UI components.
    * **`course/[courseId]`**:  Course details pages (dynamic routing).
    * **`create`**:  Course creation page.
    * **`dashboard`**:  User dashboard.
    * **`layout.js`**: Main application layout.
    * **`page.js`**: Main landing page (optional).
    * **`provider.js`**:  Clerk user provider and setup.
* **`configs`**: Configuration files:
    * **`AiModel.js`**: Configuration for Google Gemini AI models and prompts.
    * **`db.js`**: Database connection setup with Drizzle ORM.
    * **`schema.js`**: Database schema definition using Drizzle ORM.
* **`inngest`**: Inngest functions for background tasks:
    * **`client.js`**: Inngest client setup.
    * **`functions.js`**: Definitions for all Inngest functions.
* **`public`**: Static assets (images, icons, etc.).
* **`styles`**: Global stylesheets.


## API Routes

* **`/api/courses`**:  Handles fetching courses (`GET`) based on `courseId` (for individual course retrieval) or `createdBy` (for user's course list).  Also handles new course creation (`POST`) initiated by the create course page.
* **`/api/create-user`**:  An API endpoint called by the `CreateNewUser` Inngest function to create a new user record in the database upon initial login with Clerk.
* **`/api/generate-course-outline`**: Handles the creation of new courses and triggers AI course outline generation.  Receives course details (topic, type, difficulty) via `POST` request.
* **`/api/study-type`**:  Retrieves study materials for a specific course and study type (`POST`).  Handles "ALL" type to fetch all material types at once.
* **`/api/study-type-content`**:  Triggers the generation of specific study material content (Flashcards, Quiz, Q&A) through Inngest functions, using `chapter` and `type` data sent via `POST`.


## Inngest Functions

* **`helloWorld`**: Example/test function (can be removed).
* **`CreateNewUser`**: Called on user creation event; creates a new user record in the database if one doesn't exist, syncing with Clerk.
* **`GenerateNotes`**:  Triggered by `/api/generate-course-outline`; generates detailed chapter notes using the AI and updates the course status in the database.
* **`GenerateStudyTypeContent`**:  Triggered by `/api/study-type-content`; generates content for specific study material types (flashcards, quizzes, Q&A) using the configured AI models.  Updates the status of the generated content in the database.


## Contributing

Contributions are welcome!  Please follow these guidelines:

* Fork the repository.
* Create a new branch for your feature/fix.
* Commit your changes.
* Push your branch to your fork.
* Open a pull request.