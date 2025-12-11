# Biostatics-Quiz
practice make perfect
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🧪 Clinical Statistics Quiz </title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f7f6;
            color: #333;
        }

        header {
            background-color: #007BFF;
            color: white;
            padding: 20px;
            text-align: center;
            margin-bottom: 20px;
        }

        #quiz-container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            min-height: 300px; 
        }
        
        #question-counter {
            text-align: right;
            font-weight: bold;
            color: #6c757d;
            margin-bottom: 10px;
        }

        .question-card h3 {
            color: #007BFF;
            margin-top: 0;
            font-size: 1.2em;
        }

        .options-list {
            list-style: none;
            padding: 0;
        }

        .options-list li {
            margin-bottom: 8px;
            cursor: pointer;
            padding: 8px 10px;
            border-radius: 4px;
            transition: background-color 0.2s;
            border: 1px solid #ddd;
        }

        .options-list li:hover:not(.correct):not(.incorrect):not(.selected) {
            background-color: #e9ecef;
        }

        /* Answer marking styles */
        .correct {
            background-color: #d4edda !important;
            border: 1px solid #c3e6cb;
            color: #155724;
            font-weight: bold;
        }

        .incorrect {
            background-color: #f8d7da !important;
            border: 1px solid #f5c6cb;
            color: #721c24;
            position: relative;
        }
        
        /* User selection mark */
        .selected {
            background-color: #cfe2ff; 
            border: 1px solid #9fcdff;
        }

        .explanation {
            margin-top: 15px;
            padding: 15px;
            border-left: 5px solid #007BFF;
            background-color: #f0f8ff;
            display: none; 
            font-size: 0.9em;
        }
        
        .feedback {
            padding: 10px;
            margin-top: 10px;
            font-weight: bold;
            display: none;
        }
        
        .feedback.correct-feedback {
            background-color: #d4edda;
            color: #155724;
        }

        .feedback.incorrect-feedback {
            background-color: #f8d7da;
            color: #721c24;
        }


        #controls {
            text-align: center;
            margin: 20px auto;
            max-width: 800px;
        }

        #controls button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 5px;
            margin: 0 10px;
            transition: background-color 0.3s;
        }

        #controls button.submit-btn {
            background-color: #007BFF;
        }

        #controls button.next-btn {
            background-color: #ffc107;
            color: #333;
        }
        
        #controls button:hover:not(:disabled) {
            filter: brightness(1.1);
        }

        #controls button:disabled {
            background-color: #6c757d;
            cursor: not-allowed;
        }

        #results {
            max-width: 800px;
            margin: 20px auto;
            padding: 25px;
            background-color: #fff;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            text-align: center;
        }
        
        #score-display {
            font-size: 1.3em;
            font-weight: bold;
            color: #007BFF;
            margin-bottom: 15px;
        }

        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <header>
        <h1>🧪 Clinical Statistics Mock Exam (Step-by-Step)</h1>
        <p>Mode: Single question, immediate feedback, final score summary.</p>
    </header>

    <main id="quiz-container">
        <div id="question-counter"></div>
        <div id="current-question-card"></div>
        
        <div id="controls">
            <button id="submit-button" class="submit-btn" onclick="submitAnswer()">Submit Answer</button>
            <button id="next-button" class="next-btn hidden" onclick="nextQuestion()">Next Question</button>
            <button id="reset-button" onclick="resetQuiz()">Restart Quiz</button>
        </div>
    </main>
    
    <div id="results" class="hidden">
        <h2>📊 Exam Results</h2>
        <p id="score-display"></p>
        <p>Congratulations, you have completed the quiz!</p>
    </div>

    <script>
        const questions = [
            {
                question: "1. Which may happen if the comparator was studied in a different population than the NI trial?",
                options: ["A. The NI margin becomes stricter", "B. Assay sensitivity may not be given", "C. Dropouts increase type II error", "D. The study becomes blinded"],
                answer: "B",
                explanation: "If the comparator was studied in a different population, the effect size used to determine the Non-Inferiority (NI) margin might not be reliable, which can impair assay sensitivity (the ability to detect an effect when one truly exists)."
            },
            {
                question: "2. The constancy assumption in a non-inferiority trial means:",
                options: ["A. The placebo effect is constant", "B. The comparator effect remains constant", "C. The NI margin cannot change", "D. Outcomes must be identical"],
                answer: "B",
                explanation: "The constancy assumption is critical in NI trials; it assumes that the effect of the active control (comparator) observed in past trials will be similar in the current NI trial setting."
            },
            {
                question: "3. Which factor can impair assay sensitivity?",
                options: ["A. A very homogeneous population", "B. A diluted patient population", "C. A large sample size", "D. Strong blinding"],
                answer: "B",
                explanation: "A diluted patient population (e.g., including patients less likely to respond to the treatment) can reduce the true treatment effect, thereby impairing assay sensitivity."
            },
            {
                question: "4. Which is an effect measure for a binary outcome?",
                options: ["A. Mean difference", "B. Hazard ratio", "C. Risk difference", "D. Standard deviation"],
                answer: "C",
                explanation: "For binary outcomes (e.g., success/failure), the Risk Difference is a common effect measure. Mean difference and Standard Deviation are for continuous data; Hazard Ratio is for time-to-event data."
            },
            {
                question: "5. A 95% CI for A–B is (–45, 120). Which statement is correct?",
                options: ["A. A is significantly better", "B. A is significantly worse", "C. The effect is significantly > –45", "D. Superiority is demonstrated"],
                answer: "C",
                explanation: "A 95% Confidence Interval (CI) of (-45, 120) means the true difference (A–B) lies between -45 and 120. Since the lower bound is -45, the difference is significantly greater than any value less than or equal to -45. Since the CI includes 0, no significant difference (A is better or worse) or superiority is demonstrated at the 5% alpha level."
            },
            {
                question: "6. A two-sided 99% CI corresponds to one-sided α ?",
                options: ["A. 0.5%", "B. 1%", "C. 5%", "D. 10%"],
                answer: "B",
                explanation: "A two-sided 99% CI leaves $\\alpha = 100\\% - 99\\% = 1\\%$ to be split between the two tails. For a one-sided test, the significance level $\\alpha$ is concentrated in one tail, so one-sided $\\alpha = 1\\%$."
            },
            {
                question: "7. In a bioequivalence study, the one-sided α is:",
                options: ["A. 0.5%", "B. 1%", "C. 2.5%", "D. 5%"],
                answer: "D",
                explanation: "Bioequivalence (BE) studies typically use a 90% Confidence Interval (two-sided $\\alpha = 10\\%$), which corresponds to two simultaneous one-sided tests at $\\alpha = 5\\%$."
            },
            {
                question: "8. What is the Type I error rate?",
                options: ["A. Accepting a false H0", "B. Rejecting a true H0", "C. Missing a real effect", "D. Making two means differ"],
                answer: "B",
                explanation: "Type I error is the error of rejecting the null hypothesis ($H_0$) when it is actually true (a false positive)."
            },
            {
                question: "9. If SD increases while n stays constant, power will:",
                options: ["A. Increase", "B. Decrease", "C. Stay the same", "D. Become unstable"],
                answer: "B",
                explanation: "Power is the probability of detecting a real effect. An increase in Standard Deviation (SD) means higher variability in the data, which reduces the test statistic and makes it harder to find a significant difference, thus decreasing power."
            },
            {
                question: "10. If effect size doubles, required sample size:",
                options: ["A. Doubles", "B. Triples", "C. Halves", "D. Reduces by factor of 4"],
                answer: "D",
                explanation: "The required sample size ($n$) is inversely proportional to the square of the effect size ($\\Delta$). If the effect size doubles, $n$ reduces by $1 / 2^2 = 1/4$ (reduces by a factor of 4)."
            },
            {
                question: "11. NI margin –10 corresponds to superiority assumption:",
                options: ["A. Effect = 5", "B. Effect = 10", "C. Effect = –10", "D. None"],
                answer: "B",
                explanation: "The Non-Inferiority (NI) margin, $M$, is often based on the historical difference, $D$, of the active comparator versus placebo. The NI margin ($M$) is usually a fraction (e.g., 50%) of $D$. If $M=10$, it relates to the full effect size $D=10$ (Effect = 10) for superiority determination."
            },
            {
                question: "12. Effect increases 10 → 20, sample size:",
                options: ["A. Doubles", "B. Triples", "C. Halves", "D. Reduces by factor of 4"],
                answer: "D",
                explanation: "As in question 10, the sample size is inversely proportional to the square of the effect size. Doubling the effect size (10 $\\rightarrow$ 20) reduces the required sample size by a factor of 4."
            },
            {
                question: "13. Which estimand counts dropout as failure?",
                options: ["A. Hypothetical", "B. Treatment-policy", "C. Composite", "D. Principal-stratum"],
                answer: "C",
                explanation: "A Composite estimand combines a clinical outcome with an intercurrent event like discontinuation (dropout). By counting dropout as a failure, it becomes part of the composite outcome."
            },
            {
                question: "14. Which is an effect measure for binary outcome?",
                options: ["A. Hazard ratio", "B. Mean difference", "C. Risk difference", "D. Standard deviation"],
                answer: "C",
                explanation: "Risk Difference (RD) is a common effect measure for binary outcomes."
            },
            {
                question: "15. Cox model estimates:",
                options: ["A. Odds ratio", "B. Hazard ratio", "C. Risk ratio", "D. Event rate"],
                answer: "B",
                explanation: "The Cox Proportional Hazards model is used in survival analysis to estimate the Hazard Ratio (HR)."
            },
            {
                question: "16. Log-rank test compares:",
                options: ["A. Means", "B. Survival functions", "C. Risk differences", "D. Variances"],
                answer: "B",
                explanation: "The Log-rank test is a non-parametric hypothesis test used to compare the survival distributions (survival functions) of two or more groups."
            },
            {
                question: "17. If risk = 20% vs 33.3%, OR =",
                options: ["A. 1", "B. 1.5", "C. 2", "D. 3"],
                answer: "C",
                explanation: "Risk 1 ($P_1$) = 20% = 0.20, Risk 2 ($P_2$) = 33.3% $\\approx 1/3$. The Odds Ratio ($OR$) is calculated as: $OR = \\frac{P_1 / (1-P_1)}{P_2 / (1-P_2)} = 0.5$ (or $OR = 2.0$ for the inverse ratio). Based on the provided answer C, the calculated ratio is 2.0."
            },
            {
                question: "18. If OR CI includes 1, conclusion:",
                options: ["A. Significant", "B. Not significant", "C. Harmful", "D. Beneficial"],
                answer: "B",
                explanation: "For a ratio measure like the Odds Ratio (OR), a Confidence Interval (CI) that includes 1 indicates that the effect is not statistically significant, as 1 implies no difference between the groups."
            },
            {
                question: "19. If SD increases and n stays constant, power:",
                options: ["A. Increases", "B. Decreases", "C. Same", "D. Unstable"],
                answer: "B",
                explanation: "An increase in Standard Deviation (SD) increases variability, making it harder to detect a true difference, thereby decreasing power."
            },
            {
                question: "20. BE usual one-sided α =",
                options: ["A. 0.5%", "B. 1%", "C. 2.5%", "D. 5%"],
                answer: "D",
                explanation: "Bioequivalence (BE) studies typically use a one-sided $\\alpha = 5\\%$."
            },
            {
                question: "21. Frequentist statistics evaluates:",
                options: ["A. P(H|data)", "B. P(data|H)", "C. Prior probability", "D. Posterior probability"],
                answer: "B",
                explanation: "Frequentist statistics focuses on the probability of observing the data ($P(\\text{data})$) or more extreme data, given that the null hypothesis ($H_0$) is true ($P(\\text{data}|H)$)."
            },
            {
                question: "22. Which estimand counts dropout as failure?",
                options: ["A. Treatment-policy", "B. Hypothetical", "C. Composite", "D. Principal-stratum"],
                answer: "C",
                explanation: "The Composite estimand combines the desired clinical outcome with a negative intercurrent event like dropout, effectively counting the dropout as a failure."
            },
            {
                question: "23. Historical lower bound = 10 $\\rightarrow$ NI margin:",
                options: ["A. 1", "B. 2", "C. 5", "D. 10"],
                answer: "C",
                explanation: "Following the common practice of setting the Non-Inferiority (NI) margin ($M$) to 50% of the historical effect: $50\\% \\times 10 = 5$."
            },
            {
                question: "24. Historical lower bound = 20 $\\rightarrow$ NI margin:",
                options: ["A. 20", "B. 15", "C. 10", "D. 5"],
                answer: "C",
                explanation: "Following the common practice of setting the NI margin to 50% of the historical effect: $50\\% \\times 20 = 10$."
            },
            {
                question: "25. 95% CI excludes 0 means:",
                options: ["A. Not significant", "B. Significant", "C. Crosses NI", "D. Harmful"],
                answer: "B",
                explanation: "For difference measures, a 95% Confidence Interval (CI) that excludes 0 indicates that the difference is statistically significant at the $\\alpha=0.05$ level."
            },
            {
                question: "26. CI (–45, 120) means:",
                options: ["A. A > B significantly", "B. A < B significantly", "C. Effect > –45 significantly", "D. Superiority shown"],
                answer: "C",
                explanation: "The CI is for A-B. Since the lower bound is -45, the true difference is greater than -45 with 95% confidence. As the interval includes 0, there is no statistically significant difference between A and B, nor is superiority shown."
            },
            {
                question: "27. Two tests $\\alpha=0.05$, P($\\ge$1 sig) =",
                options: ["A. 0.05", "B. 0.10", "C. 0.0975", "D. 0.50"],
                answer: "C",
                explanation: "For two independent tests, the family-wise error rate (Probability of at least one significant result) is $1 - (1 - \\alpha)^2 = 1 - (1 - 0.05)^2 = 1 - 0.9025 = 0.0975$."
            },
            {
                question: "28. Which is Type I error?",
                options: ["A. Missing a real effect", "B. Detecting an effect that doesn't exist", "C. Accepting false H0", "D. Failing to reject false H0"],
                answer: "B",
                explanation: "Type I error is 'rejecting a true null hypothesis' ($H_0$), which means detecting an effect when none truly exists (a false positive)."
            },
            {
                question: "29. Which is Type II error?",
                options: ["A. Rejecting true H0", "B. Detecting true effect", "C. Failing to reject false H0", "D. Using strict alpha"],
                answer: "C",
                explanation: "Type II error is 'failing to reject a false null hypothesis' ($H_0$), which means missing a real effect (a false negative)."
            }
        ];

        let currentQuestionIndex = 0;
        let score = 0;
        let selectedAnswer = null;
        let isAnswered = false;

        const questionCardContainer = document.getElementById('current-question-card');
        const questionCounter = document.getElementById('question-counter');
        const submitButton = document.getElementById('submit-button');
        const nextButton = document.getElementById('next-button');
        const resultsDiv = document.getElementById('results');
        const quizContainer = document.getElementById('quiz-container');

        // Render the current question
        function renderQuestion() {
            if (currentQuestionIndex >= questions.length) {
                showFinalResults();
                return;
            }

            const q = questions[currentQuestionIndex];
            isAnswered = false;
            selectedAnswer = null;
            
            // Update counter
            questionCounter.textContent = `Question ${currentQuestionIndex + 1} of ${questions.length}`;

            // Handle LaTeX (alpha, ge) replacement for display
            const questionText = q.question.replace(/\\alpha/g, '$\\alpha$').replace(/\\ge/g, '$\\ge$');
            
            let html = `
                <div class="question-card" data-index="${currentQuestionIndex}">
                    <h3>${questionText}</h3>
                    <ul class="options-list">
                        ${q.options.map(option => {
                            const optionText = option.replace(/\\alpha/g, '$\\alpha$').replace(/\\ge/g, '$\\ge$');
                            return `<li data-option="${option.charAt(0)}" onclick="selectOption('${option.charAt(0)}', this)">${optionText}</li>`;
                        }).join('')}
                    </ul>
                    <div class="feedback" id="feedback-${currentQuestionIndex}"></div>
                    <div class="explanation" id="explanation-${currentQuestionIndex}">
                        <strong>Correct Answer: ${q.answer}</strong><br>
                        <strong>Explanation:</strong> ${q.explanation}
                    </div>
                </div>
            `;
            
            questionCardContainer.innerHTML = html;
            
            // Hide result area and set control buttons
            resultsDiv.classList.add('hidden');
            nextButton.classList.add('hidden');
            submitButton.classList.remove('hidden');
            submitButton.disabled = true; // Disable submit until an answer is selected
            quizContainer.classList.remove('hidden');
        }

        // Handle option selection
        function selectOption(optionLetter, liElement) {
            if (isAnswered) return; 
            
            const options = questionCardContainer.querySelectorAll('.options-list li');
            
            // Clear selection state from other options
            options.forEach(li => li.classList.remove('selected'));
            
            // Set current selection state
            liElement.classList.add('selected');

            selectedAnswer = optionLetter;
            submitButton.disabled = false; // Enable submit button
        }

        // Submit and check answer
        function submitAnswer() {
            if (isAnswered || selectedAnswer === null) return;
            
            isAnswered = true;
            const q = questions[currentQuestionIndex];
            const isCorrect = selectedAnswer === q.answer;
            const options = questionCardContainer.querySelectorAll('.options-list li');
            const feedbackDiv = document.getElementById(`feedback-${currentQuestionIndex}`);
            const explanationDiv = document.getElementById(`explanation-${currentQuestionIndex}`);

            if (isCorrect) {
                score++;
                feedbackDiv.textContent = "🎉 Correct! Well done.";
                feedbackDiv.className = "feedback correct-feedback";
            } else {
                feedbackDiv.textContent = "❌ Incorrect. Please review the explanation.";
                feedbackDiv.className = "feedback incorrect-feedback";
            }
            
            // Mark correct/incorrect answers and disable clicking
            options.forEach(li => {
                li.onclick = null; // Disable clicking
                li.classList.remove('selected'); // Remove selection highlight
                const optionLetter = li.getAttribute('data-option');
                
                if (optionLetter === q.answer) {
                    li.classList.add('correct'); // Correct answer
                } else if (optionLetter === selectedAnswer) {
                    li.classList.add('incorrect'); // User's incorrect answer
                }
            });
            
            explanationDiv.style.display = 'block'; // Show explanation
            feedbackDiv.style.display = 'block';    // Show feedback

            // Update control buttons
            submitButton.classList.add('hidden');
            nextButton.classList.remove('hidden');
            
            // If it's the last question, change "Next Question" to "View Results"
            if (currentQuestionIndex === questions.length - 1) {
                nextButton.textContent = "View Final Results";
                nextButton.onclick = showFinalResults;
            } else {
                nextButton.textContent = "Next Question";
                nextButton.onclick = nextQuestion;
            }
            
            // Scroll to the question card
            questionCardContainer.scrollIntoView({ behavior: 'smooth' });
        }

        // Move to the next question
        function nextQuestion() {
            currentQuestionIndex++;
            renderQuestion();
        }

        // Show final score
        function showFinalResults() {
            const totalQuestions = questions.length;
            const scoreDisplay = document.getElementById('score-display');
            
            scoreDisplay.innerHTML = `You answered **${score}** out of **${totalQuestions}** questions correctly.<br>Score Percentage: **${(score / totalQuestions * 100).toFixed(2)}%**`;
            
            quizContainer.classList.add('hidden');
            resultsDiv.classList.remove('hidden');
            resultsDiv.scrollIntoView({ behavior: 'smooth' });
        }

        // Reset the quiz
        function resetQuiz() {
            currentQuestionIndex = 0;
            score = 0;
            selectedAnswer = null;
            isAnswered = false;
            nextButton.textContent = "Next Question";
            renderQuestion();
        }

        // Execute on page load
        window.onload = resetQuiz;
    </script>
</body>
</html>
