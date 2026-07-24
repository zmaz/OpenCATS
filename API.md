## Using the API

```bash
# Add a candidate with a resume, parsed the same way the UI parses it
curl -X POST "http://localhost:8080/index.php?m=api&a=addCandidate" \
  -H "Authorization: Bearer <your API_KEY>" \
  -F "firstName=Supreme" \
  -F "lastName=Test" \
  -F "email1=test@example.com" \
  -F "resume=@/path/to/tailored_resume.pdf"

# Fetch a candidate back
curl "http://localhost:8080/index.php?m=api&a=getCandidate&candidateID=1" \
  -H "Authorization: Bearer <your API_KEY>"
```

The `addCandidate` response includes `resume.extractedText` — the exact
text CandidATS's own parser (pdftotext/antiword/etc. under the hood)
pulled from your file. That's your ground truth for "did this resume
parse cleanly," no screenshot-scraping required.


## Security notes

- This endpoint bypasses normal session login by design (it's meant for
  scripts, not browsers) — the API key is the *only* thing standing
  between the internet and `candidates.add`-level access. Don't expose
  port 8080 (or whatever you map it to) outside your local machine /
  trusted network unless you put this behind HTTPS and a reverse proxy.
- Treat `config/api.php` like a secrets file: `.gitignore` it, never
  commit the real key, rotate it if you ever suspect it leaked.
