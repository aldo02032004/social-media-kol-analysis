# Social Media KOL Analysis

One Google Colab notebook that turns an Indonesian media-monitoring export (news and social media mentions) into the top issues in news vs social media, and the key opinion leaders (KOLs) of a topic with their stance.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aldo02032004/social-media-kol-analysis/blob/main/social_media_kol_analysis.ipynb)

## Pipeline

1. **Clean**: separate news outlets from organic accounts, merge spellings of the same account (`@kompascom`, `kompas.com`), strip RT/reply markers, turn emoji into words, flag copypasta posted by several accounts.
2. **Top issues**: the 5 news and 5 social posts with the most followers become 5 news issues and 5 social issues, each with a sentiment score, key opinion leader and public figures.
3. **Classify**: posts without a topic keyword go straight to `lainnya`; the rest get one LLM call that decides relevance, theme, sentiment and stance toward the target. Results are cached on Drive.
4. **Network**: a retweet / reply / mention graph per theme (NetworkX). KOL score = mean percentile of PageRank, eigenvector centrality, engagement and followers.
5. **Summaries**: each KOL's stance and narrative, then a summary per theme with the supporter, moderate and opposition narratives.

## Example results

Topic: *President Prabowo's visit to Vladivostok, Russia, and his meeting with Vladimir Putin*. Stance is measured toward the president and the government. The LLM built three themes; the table counts the top 10 KOLs of each theme. Private accounts are not named here.

| Theme | Supporter | Opposition | No stance | Main narrative |
|---|---|---|---|---|
| Presidential visit | 6 | 0 | 4 | Supporters frame the trip as diplomacy, downstream industry and fuel security. |
| Meeting with Putin | 1 | 7 | 2 | Critics focus on how Prabowo answered Putin's questions and on Indonesia joining the BoP. |
| International politics | 2 | 8 | 0 | Prabowo is portrayed as having no firm position and following other countries' interests. |

What the run shows:

- **The visit itself was received well, the meeting was not.** Supportive accounts talk about energy and investment deals; the meeting with Putin drew criticism of communication style.
- **Signs of a coordinated narrative.** In *International politics*, 6 of the 10 top accounts post near-identical text ("follows the China-Russia and US-Israel gangs", "no dignity", "will bring ruin"). In *Presidential visit*, 4 accounts post the same question about Prabowo's consistency on Palestine. The exact-match copypasta check does not flag them because the wording is slightly paraphrased.
- **One account leads two themes.** The same large account (~85k followers) is the top KOL in both *Presidential visit* and *International politics*, with the highest PageRank in both networks.

### Limitations seen in this run

- **Thin data.** Almost every KOL has one relevant post, so the ranking mostly reflects position in a small reply/retweet network. An account with 3 followers ranks third in one theme.
- **Some stance labels are wrong.** One account is summarised as criticising the president but labelled *supporter*; posts questioning the president's consistency are labelled *no stance*.
- **Off-topic posts got through.** Posts about a cabinet reshuffle and a parliament leader were marked relevant.
- **News pages on social platforms are not always caught.** Two regional news outlets posting on social media appear in the network, one of them in a KOL list.
- **Summaries can drift.** A 9B model sometimes adds details that are not in the post.

## Run it

- Google Colab with a **T4 GPU** runtime. The LLM is [Gemma 2 9B CPT SahabatAI Instruct](https://huggingface.co/faizonly5953/gemma2-9b-cpt-sahabatai-v1-instruct-GGUF) (GGUF `q4_k_m`, ~5.8 GB) run with llama-cpp; answers are forced into JSON schemas. No API key.
- Input: a monitoring-tool export (Excel, Google Sheets or Drive link, shared as *Anyone with the link*) with the [GREAT-Tools](https://github.com/azmkto/GREAT-Tools) export columns (`Headline`, `Mentions`, `Media`, `Author`, `Followers`, `Retweeted`, `Favourited`, ...).
- Fill in the **Form** cell (topic, stance target, target accounts, links), then **Runtime → Run all**. The topic config and classification cache are saved to Google Drive, so a rerun after a disconnect continues where it stopped.

## License

MIT, see [LICENSE](LICENSE).
