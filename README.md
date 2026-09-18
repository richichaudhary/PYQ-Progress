# PYQ-Progress
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PYQ Study Tracker</title>
    <style>
        :root {
            --primary: #4F46E5;
            --bg: #F9FAFB;
            --surface: #FFFFFF;
            --text: #1F2937;
            --border: #E5E7EB;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg);
            color: var(--text);
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        .header {
            text-align: center;
            margin-bottom: 30px;
        }
        .progress-container {
            background-color: var(--surface);
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
            margin-bottom: 30px;
        }
        .progress-bar-bg {
            background-color: var(--border);
            height: 20px;
            border-radius: 10px;
            overflow: hidden;
            margin-top: 10px;
        }
        .progress-bar-fill {
            background-color: var(--primary);
            height: 100%;
            width: 0%;
            transition: width 0.3s ease;
        }
        .chapter-card {
            background-color: var(--surface);
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        }
        .chapter-title {
            margin-top: 0;
            border-bottom: 2px solid var(--border);
            padding-bottom: 10px;
        }
        .question-item {
            padding: 15px 0;
            border-bottom: 1px solid var(--border);
        }
        .question-item:last-child {
            border-bottom: none;
        }
        .question-header {
            display: flex;
            align-items: flex-start;
            gap: 15px;
        }
        input[type="checkbox"] {
            width: 20px;
            height: 20px;
            margin-top: 3px;
            cursor: pointer;
        }
        .question-text {
            flex-grow: 1;
            font-size: 1.05em;
        }
        .insight-box {
            margin-top: 10px;
            margin-left: 35px;
        }
        textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid var(--border);
            border-radius: 5px;
            resize: vertical;
            min-height: 60px;
            font-family: inherit;
        }
        .save-btn {
            background-color: var(--primary);
            color: white;
            border: none;
            padding: 5px 15px;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 5px;
            font-size: 0.9em;
        }
        .save-btn:hover {
            background-color: #4338CA;
        }
    </style>
</head>
<body>

    <div class="header">
        <h1>Discrete Structure PYQs Study Tracker</h1>
      

    <div class="progress-container">
        <h2>Overall Progress</h2>
        <p id="progress-text">0 / 0 Questions Completed</p>
        <div class="progress-bar-bg">
            <div class="progress-bar-fill" id="progress-fill"></div>
        </div>
    </div>

    <div id="tracker-content"></div>

   function renderTracker() {
            const container = document.getElementById('tracker-content');
            container.innerHTML = '';

            trackerData.forEach((chapter, chapterIndex) => {
                const chapterCard = document.createElement('div');
                chapterCard.className = 'chapter-card';

                const chapterTitle = document.createElement('h3');
                chapterTitle.className = 'chapter-title';
                chapterTitle.innerText = chapter.chapterName;
                chapterCard.appendChild(chapterTitle);

                chapter.questions.forEach((q, qIndex) => {
                    const qItem = document.createElement('div');
                    qItem.className = 'question-item';

                    // If an image URL is provided, render it above the text
                    if (q.imageUrl && q.imageUrl.trim() !== "") {
                        const img = document.createElement('img');
                        img.src = q.imageUrl;
                        img.style.maxWidth = "100%";
                        img.style.maxHeight = "250px"; // Keeps large graph images manageable
                        img.style.marginBottom = "10px";
                        img.style.borderRadius = "5px";
                        img.style.border = "1px solid var(--border)";
                        qItem.appendChild(img);
                    }

                    // Header (Checkbox + Question text)
                    const qHeader = document.createElement('div');
                    qHeader.className = 'question-header';

                    const checkbox = document.createElement('input');
                    checkbox.type = 'checkbox';
                    checkbox.checked = q.done;
                    checkbox.addEventListener('change', (e) => {
                        trackerData[chapterIndex].questions[qIndex].done = e.target.checked;
                        saveData();
                    });

                    const qText = document.createElement('div');
                    qText.className = 'question-text';
                    qText.innerText = q.text;

                    qHeader.appendChild(checkbox);
                    qHeader.appendChild(qText);
                    qItem.appendChild(qHeader);

                    // Insight/Notes section
                    const insightBox = document.createElement('div');
                    insightBox.className = 'insight-box';

                    const textarea = document.createElement('textarea');
                    textarea.placeholder = "Add your notes, derivations, or formulas here...";
                    textarea.value = q.insight;
                    textarea.addEventListener('input', (e) => {
                        trackerData[chapterIndex].questions[qIndex].insight = e.target.value;
                    });

                    const saveBtn = document.createElement('button');
                    saveBtn.className = 'save-btn';
                    saveBtn.innerText = "Save Note";
                    saveBtn.addEventListener('click', () => {
                        saveData();
                        alert("Note saved!");
                    });

                    insightBox.appendChild(textarea);
                    insightBox.appendChild(saveBtn);
                    qItem.appendChild(insightBox);

                    chapterCard.appendChild(qItem);
                });

                container.appendChild(chapterCard);
            });

            updateProgress();
        }
       const courseData = [
    {
        chapterName: "Chapter 1: Logic and Induction",
        questions: [
            { id: "c1q1", text: "Consider the premises: 'If I get my Dashain bonus and my friends are free then I will take a road trip with my friends.' ... Leads to the conclusion: 'I will take a road trip with my friends.' using propositional logic. [2083 Baishakh, Q1][cite: 1]", done: false, insight: "" },
            { id: "c1q2", text: "Explain different types of function. State the converse, contrapositive and inverse of the conditional statement 'My insurance company will pay me only if the flood destroys my house or the fire destroys my house.' [2082 Bhadra, Q1][cite: 1]", done: false, insight: "" },
            { id: "c1q3", text: "Check validity of the given statement using method of tableaux: A v B, A -> C, ~C v D. Hence, B -> D. [2082 Bhadra, Q4][cite: 1]", done: false, insight: "" },
            { id: "c1q4", text: "Determine whether the following system specifications are consistent or not: 'If the network is down, then users cannot access the cloud storage. If users can access the cloud storage, then they can upload files...' [2081 Chaitra, Q2][cite: 2]", done: false, insight: "" }
        ]
    },
    {
        chapterName: "Chapter 2: Proof Techniques",
        questions: [
            { id: "c2q1", text: "Prove that √2 + √3 is irrational using proof by contradiction. [2083 Baishakh, Q3][cite: 1]", done: false, insight: "" },
            { id: "c2q2", text: "Define the term counter example and witness with example. [2082 Bhadra, Q2][cite: 1]", done: false, insight: "" },
            { id: "c2q3", text: "Use the Principle of Mathematical Induction to verify that, for any positive integer n, n^3 + 2n is divisible by 3. [2082 Bhadra, Q3][cite: 1]", done: false, insight: "" },
            { id: "c2q4", text: "Use mathematical induction to prove that 7^(n+2) + 8^(2n+1) is divisible by 57 for every nonnegative integer n. [2081 Chaitra, Q3][cite: 2]", done: false, insight: "" }
        ]
    },
    {
        chapterName: "Chapter 3: Automata Theory, Regular Language and Grammar",
        questions: [
            { id: "c3q1", text: "Convert the following NDFA into equivalent DFA. [Refer to Automata Figure: 2083 Baishakh, Q5][cite: 1]", done: false, insight: "", imageUrl: "" },
            { id: "c3q2", text: "Derive the regular expression for the given automata. [Refer to Automata Figure: 2082 Bhadra, Q6][cite: 1]", done: false, insight: "", imageUrl: "" },
            { id: "c3q3", text: "Design a deterministic finite automata that accepts all the strings that does not starts with aba over Σ = {a,b}. Also check for string w1 = abbabab. [2082 Bhadra, Q5][cite: 1]", done: false, insight: "" },
            { id: "c3q4", text: "Write a regular expression for the language that accepts all the string that does not contains three consecutive b over Σ = {a,b}. Write a CFG that generates the palindrome string of even length over the alphabet Σ = {0,1}. [2083 Baishakh, Q6][cite: 1]", done: false, insight: "" },
            { id: "c3q5", text: "Consider a Regular grammar defined by (S -> bS, S -> aA, A -> bA, A -> b). Construct a FSA for this regular grammar and convert it to DFA if it is in NFA. [2081 Chaitra, Q6][cite: 2]", done: false, insight: "" }
        ]
    },
    {
        chapterName: "Chapter 4: Recurrence Relation and Algorithmic Analysis",
        questions: [
            { id: "c4q1", text: "Solve the recurrence relation a_n = 5a_{n-1} - 6a_{n-2} + 2^n with initial condition a_0 = 1 and a_1 = 4. [2083 Baishakh, Q9][cite: 1]", done: false, insight: "" },
            { id: "c4q2", text: "Write an algorithm to perform sort operation using insertion sort. Search for key = 45 from the given data 92, 87, 32, 56, 57, 45, 11, 77, 28. [2083 Baishakh, Q8][cite: 1]", done: false, insight: "" },
            { id: "c4q3", text: "Derive the worst-case time complexity for binary search algorithm. Sort the given data using bubble sort: 67, 45, 54, 2, 29. [2082 Bhadra, Q9][cite: 1]", done: false, insight: "" },
            { id: "c4q4", text: "Find all the solutions of recurrence relation: a_n = 3a_{n-1} + 4a_{n-2} + 3^n with initial conditions a_0 = 1 and a_2 = 2. [2081 Chaitra, Q7][cite: 2]", done: false, insight: "" }
        ]
    },
    {
        chapterName: "Chapter 5: Graph Theory and Tree",
        questions: [
            { id: "c5q1", text: "Find the minimum spanning tree using Prim's algorithm from the graph given below. [Refer to Graph Figure: 2083 Baishakh, Q12][cite: 1]", done: false, insight: "", imageUrl: "" },
            { id: "c5q2", text: "Find the Maximum flow for the network given below. [Refer to Flow Network Figure: 2083 Baishakh, Q13][cite: 1]", done: false, insight: "", imageUrl: "" },
            { id: "c5q3", text: "Find the minimum spanning tree using Kruskal's algorithm from the graph given below. [Refer to Graph Figure: 2082 Bhadra, Q12][cite: 1]", done: false, insight: "", imageUrl: "" },
            { id: "c5q4", text: "Find the shortest distance from vertex A to all other vertex using Dijkstra's algorithm. [Refer to Graph Figure: 2082 Bhadra, Q13][cite: 1]", done: false, insight: "", imageUrl: "" },
            { id: "c5q5", text: "Use Dijkstra's algorithm to find the cost of shortest path from vertex a to vertex z and also find the shortest path. [Refer to Graph Figure: 2081 Chaitra, Q8][cite: 2]", done: false, insight: "", imageUrl: "" },
            { id: "c5q6", text: "What are saturated edges? State Max-Cut Min Flow theorem. [2082 Bhadra, Q12][cite: 1]", done: false, insight: "" }
        ]
    }
];
        

        // Load saved data from local storage if available
        const savedData = localStorage.getItem('studyTrackerData');
        const trackerData = savedData ? JSON.parse(savedData) : courseData;

        function saveData() {
            localStorage.setItem('studyTrackerData', JSON.stringify(trackerData));
            updateProgress();
        }

        function updateProgress() {
            let total = 0;
            let completed = 0;

            trackerData.forEach(chapter => {
                chapter.questions.forEach(q => {
                    total++;
                    if (q.done) completed++;
                });
            });

            const percentage = total === 0 ? 0 : Math.round((completed / total) * 100);
            document.getElementById('progress-text').innerText = `${completed} / ${total} Questions Completed (${percentage}%)`;
            document.getElementById('progress-fill').style.width = `${percentage}%`;
        }

        function renderTracker() {
            const container = document.getElementById('tracker-content');
            container.innerHTML = '';

            trackerData.forEach((chapter, chapterIndex) => {
                const chapterCard = document.createElement('div');
                chapterCard.className = 'chapter-card';

                const chapterTitle = document.createElement('h3');
                chapterTitle.className = 'chapter-title';
                chapterTitle.innerText = chapter.chapterName;
                chapterCard.appendChild(chapterTitle);

                chapter.questions.forEach((q, qIndex) => {
                    const qItem = document.createElement('div');
                    qItem.className = 'question-item';

                    // Header (Checkbox + Question text)
                    const qHeader = document.createElement('div');
                    qHeader.className = 'question-header';

                    const checkbox = document.createElement('input');
                    checkbox.type = 'checkbox';
                    checkbox.checked = q.done;
                    checkbox.addEventListener('change', (e) => {
                        trackerData[chapterIndex].questions[qIndex].done = e.target.checked;
                        saveData();
                    });

                    const qText = document.createElement('div');
                    qText.className = 'question-text';
                    qText.innerText = q.text;

                    qHeader.appendChild(checkbox);
                    qHeader.appendChild(qText);
                    qItem.appendChild(qHeader);

                    // Insight/Notes section
                    const insightBox = document.createElement('div');
                    insightBox.className = 'insight-box';

                    const textarea = document.createElement('textarea');
                    textarea.placeholder = "Add your notes, formulas, or insights here...";
                    textarea.value = q.insight;
                    textarea.addEventListener('input', (e) => {
                        trackerData[chapterIndex].questions[qIndex].insight = e.target.value;
                    });

                    const saveBtn = document.createElement('button');
                    saveBtn.className = 'save-btn';
                    saveBtn.innerText = "Save Note";
                    saveBtn.addEventListener('click', () => {
                        saveData();
                        alert("Note saved!");
                    });

                    insightBox.appendChild(textarea);
                    insightBox.appendChild(saveBtn);
                    qItem.appendChild(insightBox);

                    chapterCard.appendChild(qItem);
                });

                container.appendChild(chapterCard);
            });

            updateProgress();
        }

        // Initialize the app
        renderTracker();
    </script>
</body>
</html>