# How to edit my blog
## Scenario 1: Adding publication
Update `_bibliography/papers.bib` file by adding bibTeX citation.
```
@inproceedings{
    abbr={EMLNLP},                      # Creates venue on the left
    title = "Pushing on Text Readability Assessment: A Transformer Meets Handcrafted Linguistic Features",
    author = "Lee, Bruce W.  and
    Jang, Yoo Sung  and
    Lee, Jason",
    editor = "Moens, Marie-Francine  and
    Huang, Xuanjing  and
    Specia, Lucia  and
    Yih, Scott Wen-tau",
    booktitle = "Proceedings of the 2021 Conference on Empirical Methods in Natural Language Processing",
    month = nov,
    year = "2021",
    address = "Online and Punta Cana, Dominican Republic",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2021.emnlp-main.834",
    doi = "10.18653/v1/2021.emnlp-main.834",
    pages = "10669--10686",
    pdf={2021.emnlp-main.834v2.pdf},    # Creates link to pdf
    selected={true},                    # Shows up in the homepage
}
```

Once the editing is done, run docker and open `localhost:8080` on a web browser.
```
docker compose up
```