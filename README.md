from flask import Flask, render_template_string, request

app = Flask(__name__)

HTML_TEMPLATE = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Color Vowels</title>
    <style>
        .vowel {
            color: red;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <h1>Color Vowels in Text</h1>
    <form method="post">
        <label for="text">Enter your text:</label><br>
        <textarea id="text" name="text" rows="4" cols="50">{{ input_text }}</textarea><br><br>
        <button type="submit">Submit</button>
    </form>
    <h2>Result:</h2>
    <p>{{ colored_text | safe }}</p>
</body>
</html>
"""

def color_vowels(text):
    vowels = "aeiouAEIOU"
    colored_text = ""
    for char in text:
        if char in vowels:
            colored_text += f'<span class="vowel">{char}</span>'
        else:
            colored_text += char
    return colored_text

@app.route("/", methods=["GET", "POST"])
def index():
    input_text = ""
    colored_text = ""
    if request.method == "POST":
        input_text = request.form.get("text", "")
        colored_text = color_vowels(input_text)
    return render_template_string(HTML_TEMPLATE, input_text=input_text, colored_text=colored_text)

if __name__ == "__main__":
    app.run(debug=True)

