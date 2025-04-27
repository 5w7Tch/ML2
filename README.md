# 🧠 Fraud Detection with Machine Learning

## 🏆 Kaggle-ის კონკურსის მოკლე მიმოხილვა

პროექტი ეფუძნება Kaggle-ის კონკურსს, სადაც მიზანია საფინანსო თაღლითობის (Fraud) აღმოჩენა ტრანზაქციების მონაცემებზე დაყრდნობით. მოცემულია ორი ძირითადი მონაცემთა ნაკრები: `train_transaction.csv` და `train_identity.csv`, რომელთა გაერთიანებით უნდა განვსაზღვროთ, არის თუ არა კონკრეტული ტრანზაქცია თაღლითური.

## 🎯 ჩვენი მიდგომა

პრობლემის გადასაჭრელად გამოვიყენეთ შემდეგი სტრატეგია:
- მონაცემთა გაწმენდა და წინასწარი დამუშავება
- Feature Engineering: ახალი მახასიათებლების აგება
- Feature Selection: ყველაზე ინფორმაციული ცვლადების ამოღება
- მოდელების ტრეინინგი (XGBoost, LightGBM, LogisticRegression, NeuralNet)
- MLflow-ის გამოყენება ექსპერიმენტების ავტომატური ლოგირებისთვის

---

## 📁 რეპოზიტორიის სტრუქტურა

`model_experiment_XGBoost.ipynb` # XGBoost მოდელის ტრეინინგი 
`NeuralNet_Training.ipynb` # ნერვული ქსელის ტრეინინგი 
`LogisticRegression_Training.ipynb` # ლოგისტიკური რეგრესიის ტრეინინგი 
`LightGBM_Training.ipynb` # LightGBM მოდელის ტრეინინგი


---

## 🧱 Feature Engineering

**კატეგორიული ცვლადების რიცხვითში გადაყვანა:**
- `LabelEncoder` ყველა ობიექტის ტიპის ცვლადისთვის.

**NaN მნიშვნელობების დამუშავება:**
- რიცხვითი ცვლადები შეივსო **მედიანით**.
- კატეგორიული ცვლადები შეივსო `"missing"`-ით.

**Cleaning მიდგომები:**
- წავშალეთ ცვლადები, რომლებსაც 90%-ზე მეტი NaN ჰქონდათ.
- გაერთიანდა ორი ძირითადი CSV ფაილი `TransactionID`-ის მიხედვით.

**Feature Engineering:**
- შეიქმნა ახალი ფიჩერი: `TransactionAmt_to_mean_card1` — მიმდინარე ტრანზაქციის რაოდენობის შეფარდება card1 ჯგუფის საშუალო რაოდენობასთან.

---

## 🔍 Feature Selection

**გამოყენებული მეთოდი:**
- `SelectKBest` `mutual_info_classif` სქორით.

**მიზანი:**
- ავარჩიეთ საუკეთესო 100 ფიჩერი, რომლებიც ყველაზე მეტად დაკავშირებული იყო სამიზნე ცვლადთან (`isFraud`).

---

## 🧠 Training

**ტესტირებული მოდელები:**
- ✅ XGBoost  
- ✅ LightGBM  
- ✅ Logistic Regression  
- ✅ Neural Network (MLP)

**Hyperparameter ოპტიმიზაცია:**
- XGBoost და LightGBM მოდელებისთვის გამოყენებულ იქნა ოპტიმალური პარამეტრები ხელით დაყვანით.
  
**საბოლოო მოდელის შერჩევა:**
- LightGBM და XGBoost აჩვენეს საუკეთესო ROC AUC მეტრიკა.
- LightGBM შეირჩა საბოლოო მოდელად უკეთესი შედეგის, მარტივობისა და სწრაფი ტრენინგის გამო.

---

## 📊 MLflow Tracking

**MLflow ექსპერიმენტების ბმული:**
> https://dagshub.com/nurch22/ML2.mlflow/#/experiments/0?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D

**ჩაწერილი მეტრიკები:**
- Validation AUC (`val_auc`)
- გამოყენებული პარამეტრები (`learning_rate`, `num_leaves`, `bagging_fraction`, და სხვ.)

**საუკეთესო მოდელის შედეგები:**
- ✅ LightGBM: `val_auc ≈ 0.85`
- ყველა მოდელი ლოგირებულია `mlflow.lightgbm.log_model(model, "model")` მეთოდით.

