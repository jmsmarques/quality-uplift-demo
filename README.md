# GitHub Actions Exercises for Creating and Altering a GitHub Page

These exercises will guide you through setting up and using GitHub Actions to automate a GitHub Pages site.

---

## Exercise 1: Set Up a GitHub Page
**Objective**: Create a simple GitHub Page from an HTML file and use GitHub Actions to deploy updates.

### Steps:
1. Create a new repository named `github-pages-actions`.
2. Add a basic `index.html` file in the root folder.
```<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DevOps Study Hub</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #1e1e2f;
            color: #f5f5f5;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 800px;
            background: #28293d;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
            padding: 30px;
            text-align: center;
        }

        h1 {
            font-size: 2.5rem;
            color: #00c7b7;
            margin-bottom: 20px;
        }

        p {
            font-size: 1.1rem;
            line-height: 1.6;
            margin-bottom: 20px;
        }

        .code-snippet {
            background-color: #1b1b32;
            color: #00c7b7;
            padding: 15px;
            border-radius: 5px;
            font-family: 'Courier New', Courier, monospace;
            text-align: left;
            margin: 20px 0;
        }

        .btn {
            display: inline-block;
            background-color: #00c7b7;
            color: #28293d;
            text-decoration: none;
            padding: 10px 20px;
            border-radius: 5px;
            font-weight: bold;
            transition: background-color 0.3s;
        }

        .btn:hover {
            background-color: #019d8a;
        }

        .footer {
            margin-top: 20px;
            font-size: 0.9rem;
            color: #888;
        }

        .highlight {
            color: #ff6347;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Welcome to the DevOps Study Hub</h1>
        <p>
            Embark on a journey to master the art of <span class="highlight">continuous integration</span> 
            and <span class="highlight">continuous deployment</span>. Embrace the DevOps philosophy, 
            where collaboration and automation create seamless delivery pipelines.
        </p>
        <div class="code-snippet">
            # Example: Automating Deployment with GitHub Actions<br>
            name: CI/CD Pipeline<br>
            on: [push]<br>
            jobs:<br>
            &nbsp;&nbsp;build:<br>
            &nbsp;&nbsp;&nbsp;&nbsp;runs-on: ubuntu-latest<br>
            &nbsp;&nbsp;&nbsp;&nbsp;steps:<br>
            &nbsp;&nbsp;&nbsp;&nbsp;- name: Checkout code<br>
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;uses: actions/checkout@v3<br>
            &nbsp;&nbsp;&nbsp;&nbsp;- name: Run tests<br>
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;run: npm test
        </div>
        <a href="https://github.com" class="btn" target="_blank">Learn More</a>
        <div class="footer">
            &copy; 2024 DevOps Study Hub | Keep automating, keep innovating.
        </div>
    </div>
</body>
</html>
```
3. Configure the repository settings to enable GitHub Pages and point it `index.html`.
4. Access the page in https://<your-username>.github.io/github-pages-actions/.

**Success Criteria**: The site should automatically deploy with the latest version of `index.html` upon any change to the `main` branch.

---

## Exercise 2: Automate README creation
**Objective**: Use GitHub Actions to automatically create a README from the index html.

### Steps:
1. Create a pipeline named `readme.yaml`.
2. Write a GitHub Action that reads the contents of `index.html` and creates a README based on its contents.
```
node -e "
const fs = require('fs');
const { JSDOM } = require('jsdom');

// Load index.html
const htmlContent = fs.readFileSync('index.html', 'utf-8');
const dom = new JSDOM(htmlContent);

// Extract <title> and first <p> as description
const title = dom.window.document.querySelector('title')?.textContent || 'My Project';
const description = dom.window.document.querySelector('p')?.textContent || 'Description not available.';

// Generate README content
const readmeContent = \`# \${title}\n\n\${description}\n\nThis README was generated automatically from index.html.\`;

// Write to README.md
fs.writeFileSync('README.md', readmeContent);
"
```
3. Enable read write permissions in your repo Settings/Actions/General.
4. Run the pipeline to generate a new README.

**Success Criteria**: Automatically create a README based on `index.html` contents.

---

## Exercise 3: Create a pipeline that changes the page title based on an input
**Objective**: Integrate an HTML linter to check for code quality before deployment.

### Steps:
1. Write a GitHub Action changes the page title based on an input.
2. Ensure the worflow is capable of changing the title.

**Success Criteria**: The GitHub Action should be able to change the HTML file.

---

## Exercise 4: Implement a Validation Workflow
**Objective**: Ensure deployment only happens if all tests pass.

### Steps:
1. Write a simple test script to check for the presence of a title tag in `index.html`.
2. Modify the GitHub Actions workflow to run the linter and test script.
3. Ensure the site is deployed only when both the linter and test script pass.

**Success Criteria**: Deployment occurs only if the HTML linter and test script pass.

---

## Exercise 5: Create a Blue/Green Deployment
**Objective**: Implement a pipeline that changes `index.html` contents than `index_v2.html`.

### Steps:
1. Create a new html file called `index_v2.html`.
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>How Much Do You Like DevOps?</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: #f4f4f4;
        }
        h1 {
            color: #4CAF50;
            margin-bottom: 20px;
        }
        .container {
            text-align: center;
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
        }
        select {
            font-size: 16px;
            padding: 5px 10px;
            margin-top: 10px;
        }
        footer {
            margin-top: 20px;
            color: #888;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>How Much Do You Like DevOps?</h1>
        <label for="devops-level">Choose your level of enthusiasm:</label>
        <br>
        <select id="devops-level" name="devops-level">
            <option value="not-much">Not Much 😐</option>
            <option value="somewhat">Somewhat 🙂</option>
            <option value="love-it">I Love It! 😃</option>
            <option value="cant-live-without">Can't Live Without It! 🚀</option>
        </select>
        <footer>Make your choice and embrace the DevOps culture!</footer>
    </div>
</body>
</html>

```
2. Create a workflow that swaps the contents of the 2 files named `bluegreen_swap.yaml`.
3. Ensure page can be swapped using the pipeline.

**Success Criteria**: The contents of `index.html` and `index_v2.html` can be swapped using the `bluegreen_swap.yaml` pipeline. 

---

Copy these exercises into your project documentation to guide your work with GitHub Actions and GitHub Pages.
