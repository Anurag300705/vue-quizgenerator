## Project Components

This project is a Vue 3 quiz generator app that uses the Google Gemini API to generate quiz questions based on a user-supplied topic. The main components are:

### 1. [`StartScreen`](src/components/StartScreen.vue)

- **Purpose:**  
  Allows the user to enter a topic for the quiz.
- **Features:**  
  - Input field for the quiz topic.
  - "Start Quiz" button (disabled until a topic is entered).
  - Emits a `start-quiz` event with the topic when the button is clicked.

### 2. [`Loader`](src/components/Loader.vue)

- **Purpose:**  
  Displays a loading animation while quiz questions are being generated.
- **Features:**  
  - Simple spinner animation.
  - Shown when the app is waiting for the AI to return questions.

### 3. [`Quiz`](src/components/Quiz.vue)

- **Purpose:**  
  Presents quiz questions to the user and collects their answers.
- **Features:**  
  - Displays the current question and multiple-choice answers.
  - Randomizes the order of answer options.
  - Tracks the current question index.
  - Emits `store-answer` with the user's answer after each question.
  - Emits `end-quiz` when the last question is answered.

### 4. [`Result`](src/components/Result.vue)

- **Purpose:**  
  Shows the user's quiz results.
- **Features:**  
  - Displays a congratulatory or encouragement message based on performance.
  - Shows the number of correct answers.
  - Lists each question, the user's answer, and the correct answer, with visual feedback for correct/incorrect responses.

### 5. [`App`](src/App.vue)

- **Purpose:**  
  The main application component that manages state and orchestrates the flow between screens.
- **Features:**  
  - Handles quiz state (`start`, `loading`, `ready`, `finished`).
  - Calls the Google Gemini API to generate quiz questions.
  - Stores user answers and passes them to the result screen.
  - Renders the appropriate component based on the current state.

---

## Procedures

### 1. **Installation**

```sh
npm install
```

### 2. **Environment Setup**

- Add your Google Gemini API key to a `.env` file:
  ```
  API_KEY="your-api-key-here"
  ```

### 3. **Running the App**

```sh
npm run dev
```
- Open the local server URL shown in your terminal.

### 4. **Using the App**

1. **Enter a Topic:**  
   On the start screen, type a topic (e.g., "space", "history") and click "Start Quiz".
2. **Wait for Questions:**  
   The loader will display while questions are generated.
3. **Answer Questions:**  
   Select an answer for each question and click "Send" to proceed.
4. **View Results:**  
   After the last question, your results will be shown, including which answers were correct or incorrect.

---

## File Structure

- [`src/components/StartScreen.vue`](src/components/StartScreen.vue)  
- [`src/components/Loader.vue`](src/components/Loader.vue)  
- [`src/components/Quiz.vue`](src/components/Quiz.vue)  
- [`src/components/Result.vue`](src/components/Result.vue)  
- [`src/App.vue`](src/App.vue)  

Refer to the code in each file for further customization
