# Vesuvius

https://www.kaggle.com/competitions/vesuvius-challenge-ink-detection

## Goal is to detect ink in papyrus scrolls (Micro-CT-Scans)
## Challenge: Large 3D Volumes & 2D Labels

## Notebooks:
1) EDA
2) Data Preprocessing
  -> Best way of preprocessing was cropping large regions out that had ink in it
  -> Only use the middle region (surface / bottom prob. dont contain much ink - see also Notebook 6)
  -> Cropping smaller regions out made training unstable (prob. because many region hadnt any ink in them + dice loss)
3) Training
  -> 3-Fold-CV: 3 Scans; each is the valid fold once (large difference in samples made this not the ideal way)
  -> Used 2.5D CNN and a modified ResNet34
  -> 3D worked slightly better but is really expensive with large crops (batch_size=2 was max. for 24GB VRAM)
  -> HP were optimized in another Notebook (ToDo: Make optuna work with CV)
4) Visualize predictions
  -> Predict the whole scan with the model that had this fragment as valdation
  -> Some letters become readable for fragments 1 and 3
  -> Second model had bad results (prob. because the training data was a lot smaller)
5) Analyse the depth of the ink
  -> Ink is mainly in the middle which was proven indirectly
  -> Idea: Train a model with the optimized HP only on defined layers and evaluate performance
    -> If the model learns more it indicates that more information (i.e. ink) was available in the regions
