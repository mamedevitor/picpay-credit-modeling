## 2. Metodologia & Premissas

- **Divisão DEV / OOT**  
  Separação tempo-estratificada por safra para simular produção vs. validação real:  
  - DEV: 201402–201409  
  - OOT: 201410–201412  

- **Tratamento de faltantes**  
  - Colunas com missings: preenchimento com `-1` para penalizar clientes sem informações de dados.  


- **PSI (Population Stability Index)**  
  Monitoramento de estabilidade das distribuições com **bins de largura fixa**.

- **IV (Information Value)**  
  Avaliação do poder discriminativo de cada feature.

- **Seleção de features**  
  1. BorutaPy para seleção inicial.  
  2. Remoção de variáveis altamente correlacionadas (`|corr| > 0.9`).

---

## 3. Modelagem

**Notebook:** `lgbm.ipynb`  

- **Algoritmo:** LightGBM Classifier  
- **Tuning:** Optuna (exploração de `learning_rate`, `num_leaves`, `min_child_samples`, `feature_fraction`, `subsample`, `min_split_gain`, etc.)  
- **Estratégias anti-overfitting:**  
  - Early stopping em validação OOT  
  - Ajuste de `feature_fraction` e `subsample`  
  - Regularização (`reg_alpha`, `reg_lambda`)  


## 4. Avaliação de Performance

**Notebook:** `lgbm.ipynb`  

- **Cross-Validation:** StratifiedKFold (shuffle=True)  
- **Métricas:**  
  - **AUC / Gini**: CV, DEV, OOT e por safra  
  - **KS**: máxima diferença entre TPR e FPR  
  - **F1**, **Accuracy**, **MCC** em threshold=0.5, threshold=média, threshold=0.30  
  **Shifts** (variação absoluta) entre DEV e OOT  

**Notebook:** `lgbm_predict.ipynb`  
- **Interpretabilidade:**  
  - SHAP Summary Plot para valores globais e direcionais das features.  
  - Feature Importance (gain %) e ganho relativo por feature.
- **Estabilidade:**  
  - PSI por safra (top 20 features) com bins fixos.

---

## 5. Manutenibilidade & Boas Práticas

- **Controle de versão** com Git e arquivo `.gitignore` para artefatos transitórios.  
- **Dependências** listadas em `requirements.txt`.  
- **Logging & Tracking** de experimentos no MLflow (`mlruns/` ignorado).

---

## 6. Visualizações

- **A maioria em arquivos .xlsx ou nos próprios notebooks**  
  - Barplots de Feature Importance (gain %)  
  - SHAP Summary Plots  
  - Curvas ROC e KS por safra  
  - Gráficos de PSI ao longo das safras  

  - Métricas por safra (`metrics_by_safra.xlsx`)  
  - Tabelas de PSI (`psi_df.xlsx`) e IV (`iv_df.xlsx`)  
  - Tabela de ganho relativo (`fi.xlsx`)

---
