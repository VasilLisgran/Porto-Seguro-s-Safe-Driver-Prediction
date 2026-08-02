# Porto-Seguro-s-Safe-Driver-Prediction
Шаги по работе с датасетом https://www.kaggle.com/competitions/porto-seguro-safe-driver-prediction/overview

# Что было сделано?

##
0. Изначально были добавлены новые фичи, в виде данных, которые очень сильно превалируют в каких-то признаках, но являются выбросами.
1. Попробовал заменить -1 на np.nan для лучшей (на мой взгляд) обработки моделями.
2. После первой отправки были удалены из features столбцы ps_calc_ (шум)
3. Разделил для каждой модели признаки для лучшей обработки
4. Одну из ошибок ИИ предложил обработать так:
Версия для LightGBM и XGBoost: категориальные признаки как category dtype. Важно: после replace(-1, np.nan) эти колонки стали float64, поэтому сначала заполняем пропуски целочисленным кодом и приводим к int, и только потом к category — иначе XGBoost падает с ошибкой "Category index from DataFrame has floating point dtype".
``` python
X_lgb = X.copy()
X_test_lgb = X_test.copy()
X_xgb = X.copy()
X_test_xgb = X_test.copy()
for col in cat_cols:
    X_lgb[col] = X_lgb[col].fillna(-1).astype(int).astype('category')
    X_test_lgb[col] = X_test_lgb[col].fillna(-1).astype(int).astype('category')
    X_xgb[col] = X_xgb[col].fillna(-1).astype(int).astype('category')
    X_test_xgb[col] = X_test_xgb[col].fillna(-1).astype(int).astype('category')
```

Положение в топе: c 70% удалось подняться до 50%. Выше пока не понимаю как. 
Возможно, я упускаю что-то очевидное. Хотелось бы получить совет. 
P.S.: в топ не добавляет, но общее число участников 5157, я был бы на 2507
<img width="1709" height="930" alt="Снимок экрана 2026-08-02 в 14 00 10" src="https://github.com/user-attachments/assets/52871e0c-6c29-4d40-aa68-3c49ff66efa2" />
<img width="1695" height="930" alt="Снимок экрана 2026-08-02 в 13 59 30" src="https://github.com/user-attachments/assets/c128f4f3-c35b-4b75-8232-136ff306cd2a" />
