# Vesuvius

https://www.kaggle.com/competitions/vesuvius-challenge-ink-detection

## Goal is to detect ink in papyrus scrolls (Micro-CT-Scans)<br>
## Challenge: Large 3D Volumes & 2D Labels<br>

## Notebooks:<br>
1) EDA<br>
2) Data Preprocessing<br>
  -> Best way of preprocessing was cropping large regions out that had ink in it<br>
  -> Only use the middle region (surface / bottom prob. dont contain much ink - see also Notebook 6)<br>
  -> Cropping smaller regions out made training unstable (prob. because many region hadnt any ink in them + dice loss)<br>
3) Training<br>
  -> 3-Fold-CV: 3 Scans; each is the valid fold once (large difference in samples made this not the ideal way)<br>
  -> Used 2.5D CNN and a modified ResNet34<br>
  -> 3D worked slightly better but is really expensive with large crops (batch_size=2 was max. for 24GB VRAM)<br>
  -> HP were optimized in another Notebook (ToDo: Make optuna work with CV)<br>
4) Visualize predictions<br>
  -> Predict the whole scan with the model that had this fragment as valdation<br>
  -> Some letters become readable for fragments 1 and 3<br>
  -> Second model had bad results (prob. because the training data was a lot smaller)<br>
5) Analyse the depth of the ink<br>
  -> Ink is mainly in the middle which was proven indirectly<br>
  -> Idea: Train a model with the optimized HP only on defined layers and evaluate performance<br>
    -> If the model learns more it indicates that more information (i.e. ink) was available in the regions<br>
