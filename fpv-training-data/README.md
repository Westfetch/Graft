# FPV Brain — Training Data Collection Pipeline

Automated data collection from public FPV communities to build and continuously update a comprehensive FPV knowledge base. Built for the FPV Garage app.

## Sources

| Source | What it collects | Update frequency |
|--------|-----------------|------------------|
| **Reddit** | Posts + comment threads from r/fpv, r/TinyWhoop, r/Multicopter, r/fpvracing, r/diydrones, r/radiocontrol | Every 6 hours |
| **YouTube** | Video metadata + full transcripts from Joshua Bardwell, Mr Steele, Rotor Riot, UAVfutures, Oscar Liang, and more | Daily |
| **GitHub** | Issues, discussions, and wiki content from Betaflight, INAV, ExpressLRS, EdgeTX, Rotorflight repos | Daily |
| **Blogs** | Full articles from Oscar Liang, Propwashed, GetFPV Learn | Weekly |
| **Forums** | Thread content from RCGroups (Multirotor, FPV Racing), IntoFPV | Every 48 hours |

## Setup

```bash
cd fpv-training-data
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Copy and fill in API credentials
cp .env.example .env
# Edit .env with your keys
```

## Usage

```bash
# Run everything (all scrapers + processing)
python run.py

# Run individual scrapers
python run.py --reddit
python run.py --youtube
python run.py --github
python run.py --blogs
python run.py --forums

# Process raw data into training format
python run.py --process

# View collection stats
python run.py --stats

# Run continuously on schedule
python run.py --schedule
```

## Output

Processed data is exported to `data/processed/` in two formats:
- **JSONL** — one document per line, ready for fine-tuning or RAG ingestion
- **Parquet** — columnar format for analytics

Each item includes:
- Content text (cleaned and normalized)
- Source metadata (URL, author, date, scores)
- Auto-classified category (pid_tuning, build_guide, troubleshooting, gear_recommendation, etc.)
- FPV-specific tags (betaflight, elrs, tiny_whoop, etc.)
- Quality signals (upvotes, view counts, comment counts)

Category-specific files are also exported (e.g., `fpv_brain_pid_tuning.jsonl`) for targeted training.

## API Keys Required

- **Reddit**: Free — create an app at reddit.com/prefs/apps
- **YouTube**: Free tier (10,000 units/day) — Google Cloud Console
- **GitHub**: Optional but recommended — increases rate limits from 60 to 5,000 requests/hour
