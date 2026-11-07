# Social Media KOL Analysis

One Google Colab notebook that turns an Indonesian media-monitoring export (news and social media mentions) into the top issues in news vs social media, and the key opinion leaders (KOLs) of a topic with their stance.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aldo02032004/social-media-kol-analysis/blob/main/social_media_kol_analysis.ipynb)

## Pipeline

1. **Clean**: separate news outlets from organic accounts, merge spellings of the same account (`@kompascom`, `kompas.com`), strip RT/reply markers, turn emoji into words, flag copypasta posted by several accounts.
2. **Top issues**: the 5 news and 5 social posts with the most followers become 5 news issues and 5 social issues, each with a sentiment score, key opinion leader and public figures.
3. **Classify**: posts without a topic keyword go straight to `lainnya`; the rest get a theme, sentiment and stance toward the target from the LLM, plus a relevance probability read from the model's logits. Results are cached on Drive.
4. **Network**: a retweet / reply / mention graph per theme (NetworkX). KOL score = mean percentile of PageRank, eigenvector centrality, engagement and followers.
5. **Summaries**: each KOL's stance and narrative, then a summary per theme with the supporter, moderate and opposition narratives.
6. **Evaluate** (optional): label a fixed random sample by hand to get kappa, precision/recall and the best relevance threshold.

## Run it

- Google Colab with a **T4 GPU** runtime. The LLM is [Gemma 2 9B CPT SahabatAI Instruct](https://huggingface.co/faizonly5953/gemma2-9b-cpt-sahabatai-v1-instruct-GGUF) (GGUF `q4_k_m`, ~5.8 GB) run with llama-cpp; answers are forced into JSON schemas. No API key.
- Input: a monitoring-tool export (Excel, Google Sheets or Drive link, shared as *Anyone with the link*) with the [GREAT-Tools](https://github.com/azmkto/GREAT-Tools) export columns (`Headline`, `Mentions`, `Media`, `Author`, `Followers`, `Retweeted`, `Favourited`, ...).
- Fill in the **Form** cell (topic, stance target, target accounts, links), then **Runtime → Run all**. Topic config, classification cache and labels are saved to Google Drive.

## License

MIT, see [LICENSE](LICENSE).
