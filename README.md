HN Time Capsule

<img width="1212" height="721" alt="hnhero" src="https://github.com/user-attachments/assets/82187f37-ace3-47ba-9b22-b6adf0e1822f" />

**Hero project:** A Hacker News time capsule that rewinds the clock exactly **10 years**, pulls the HN frontpage of that day, and uses LLM-powered hindsight to evaluate how well predictions aged. The result? A synthesized HTML report that highlights who was *spot on* and who missed the mark.  

Also see my short blog post for additional context.  

---

🚀 What This Project Does  

- **Fetches** the Hacker News frontpage from exactly 10 years ago  
  *(example: HN frontpage from Dec 9, 2015 [(news.ycombinator.com in Bing)](https://www.bing.com/search?q="https%3A%2F%2Fnews.ycombinator.com%2Ffront%3Fday%3D2015-12-09"))*  
- **Retrieves** original article content + all HN comments  
- **Generates prompts** for an LLM to analyze outcomes with hindsight  
- **Parses responses** to extract grades for each commenter  
- **Renders** a clean HTML summary with analyses, grades, and highlights  

🎯 **Goal:** Identify which HN commenters were most prescient (or hilariously wrong), and surface predictions that aged in surprising ways.  

💡 **Big Idea:** LLMs can automatically mine human artifacts from the past and transform them into structured insights.  

---

⚡ Vibe Code Disclaimer  

This repo was **99% vibe-coded in a few hours** with Opus 4.5.  
It works, but don’t expect ongoing support. Code is provided *as-is*.  

---

## 🛠️ Setup  

```bash
# Install dependencies
uv sync

# Add your OpenAI API key
echo "OPENAI_API_KEY=your-key-here" > .env
```

---

## ▶️ Usage  

The main entry point is `pipeline.py`. It has **5 stages** that can be run individually or all at once:  

```bash
# Run all stages for today minus 10 years
uv run python pipeline.py all

# Run with a limit (for testing)
uv run python pipeline.py all --limit 5

# Run for a specific date
uv run python pipeline.py all --date 2015-06-15
```

### Run individual stages  
```bash
uv run python pipeline.py fetch      # fetch frontpage + articles + comments
uv run python pipeline.py prompt     # generate LLM prompts
uv run python pipeline.py analyze    # run LLM analysis (costs money!)
uv run python pipeline.py parse      # extract grades from responses
uv run python pipeline.py render     # generate HTML summary
```

### Use a cheaper model for testing  
```bash
uv run python pipeline.py analyze --model gpt-5-mini
```

---

## 📂 Data Directory Structure  

```
data/
  2015-12-09/
    frontpage.json        # list of all articles from that day
    all_grades.json       # aggregated grades across all articles
    summary.html          # rendered HTML report
    10699846/             # directory per article (by item_id)
      meta.json           # article metadata
      article.txt         # fetched article content
      article_error.txt   # error if fetch failed
      comments.json       # HN comment tree
      prompt.md           # full LLM prompt
      response.md         # LLM analysis output
      grades.json         # parsed grades from response
```

---

## 📜 Files  

- **pipeline.py** → Main pipeline with all stages (`clean`, `fetch`, `prompt`, `analyze`, `parse`, `render`)  

---

## ✨ Example Output  

For each article + discussion, the LLM:  

- Summarizes what actually happened over the past decade  
- Awards **“Most Prescient”** and **“Most Wrong”** badges to commenters  
- Notes fun or notable aspects of the discussion  
- Grades each commenter (A+ → F) based on how their comments aged  

Grades are aggregated into a **Hall of Fame**, tracking which HN accounts consistently nailed predictions (or bombed).  

---

## 📄 License  

MIT  

---
