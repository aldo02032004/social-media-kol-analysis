# Social Media KOL Analysis

Two Google Colab notebooks for Indonesian media-monitoring exports (news and social media mentions): clean the mentions and split news from social media, then find the key opinion leaders (KOLs) in a topic and their stance.

| Notebook | What it does | Open |
|---|---|---|
| [`mention_cleaning_media_classification.ipynb`](mention_cleaning_media_classification.ipynb) | Cleans mention text (news boilerplate, URLs, hashtags, @mentions, slang) and drops emoji-only posts, broadcast spam and duplicates. Splits rows into **social media** and **news/online**, then Gemini (structured JSON output) names the 5 top issues per category with sentiment score, key opinion leader and public figures. | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aldo02032004/social-media-kol-analysis/blob/main/mention_cleaning_media_classification.ipynb) |
| [`kol_network_stance.ipynb`](kol_network_stance.ipynb) | Builds a retweet / reply / mention network with NetworkX (PageRank, Louvain communities), scores KOLs, and uses a local Gemma LLM (llama-cpp on the Colab GPU) for per-post sentiment, each KOL's stance and summary, and per-theme narratives. Includes manual labelling and evaluation against the LLM. | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aldo02032004/social-media-kol-analysis/blob/main/kol_network_stance.ipynb) |

## Input

A monitoring-tool export (Excel, Google Sheets or Drive link) with the standard columns such as `Headline`, `Mentions`, `Media`, `Author`, `Followers`, `Retweeted`, `Favourited`.

## Requirements

- Google Colab. `kol_network_stance` needs a **T4 GPU** runtime and saves its config, cache and labels to Google Drive.
- `mention_cleaning_media_classification` needs a Gemini API key in Colab **Secrets** as `GOOGLE_API_KEY` (last step only).
- Text cleaning comes from [GREAT-Tools](https://github.com/azmkto/GREAT-Tools).

## License

MIT, see [LICENSE](LICENSE).
