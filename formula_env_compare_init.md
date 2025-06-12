Here's a simple HTML + JavaScript app that compares two `.env`-style environment files entered in text boxes and highlights the differences:

---

### ✅ Features:

* Input two `.env` files in text format.
* Compares key-value pairs.
* Shows keys missing in either file or with different values.

---

### 🔧 Code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>.env File Comparator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 2em;
    }
    textarea {
      width: 45%;
      height: 200px;
      margin-right: 2%;
      vertical-align: top;
    }
    #result {
      margin-top: 20px;
      white-space: pre-wrap;
      background: #f4f4f4;
      padding: 1em;
      border: 1px solid #ccc;
    }
    .diff { color: red; }
    .match { color: green; }
  </style>
</head>
<body>

<h2>🔍 Compare Two .env Files</h2>
<textarea id="env1" placeholder="Paste first .env file here..."></textarea>
<textarea id="env2" placeholder="Paste second .env file here..."></textarea>
<br><br>
<button onclick="compareEnvFiles()">Compare</button>

<div id="result"></div>

<script>
  function parseEnv(content) {
    const lines = content.split('\n');
    const env = {};
    for (let line of lines) {
      line = line.trim();
      if (!line || line.startsWith('#')) continue;
      const [key, ...rest] = line.split('=');
      if (key) env[key.trim()] = rest.join('=').trim();
    }
    return env;
  }

  function compareEnvFiles() {
    const env1 = parseEnv(document.getElementById('env1').value);
    const env2 = parseEnv(document.getElementById('env2').value);
    let output = '';

    const allKeys = new Set([...Object.keys(env1), ...Object.keys(env2)]);

    allKeys.forEach(key => {
      const val1 = env1[key];
      const val2 = env2[key];

      if (val1 === undefined) {
        output += `🔺 <span class="diff">${key} only in second file</span>\n`;
      } else if (val2 === undefined) {
        output += `🔻 <span class="diff">${key} only in first file</span>\n`;
      } else if (val1 !== val2) {
        output += `⚠️ <span class="diff">${key} differs:</span>\n  First: ${val1}\n  Second: ${val2}\n\n`;
      } else {
        output += `✅ <span class="match">${key} matches</span>\n`;
      }
    });

    document.getElementById('result').innerHTML = output;
  }
</script>

</body>
</html>
```

---

### 🧪 Example Usage:

1. Paste `.env` file content into both boxes:

   ```
   API_KEY=123
   DB_HOST=localhost
   ```
2. Click **Compare**.
3. Results show what’s matched or different.

---

Would you like me to add export or download as `.diff`/`.txt` feature?
