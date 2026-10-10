# Proiectul #1 - Telco Customer Churn

**Setul de date:** Telco Customer Churn (Kaggle), 7.043 de clienți, 21 de coloane.
**Ce prezicem:** dacă un client părăsește operatorul (`Churn` = Yes/No).
**Notebook:** [proiect_1_telco_churn.ipynb](proiect_1_telco_churn.ipynb)

# 1. E clasificare sau regresie? Ce greșeală a modelului ar costa mai mult în realitate?

- Avem de prezis DA/NU, prim urmare e o clasificare. Regresia se aplică la prezicerea unor valori numerice.
- Mai grav e dacă modelul prezice că pleacă și noi facem o reducere pentru a nu pierde clientul, există șansa ca el să rămână, dar dacă el prezice că rămâne și în realitate clientul pleacă practic modelul nu a fost eficient și ne costă un client pierdut. 

# 2. De ce împărțim datele înainte de scalare?

Împărțim datele înainte de scalare pentru că scalarea învață media și deviația, iar asta trebuie să o facă doar din datele de antrenare. Datele de test sunt practic clienții noi, pe care modelul nu i-a văzut niciodată, așa că nu trebuie să influențeze nimic din ce învață modelul. Dacă am scala înainte să împărțim, informații din test ar ajunge și în antrenare și scorul ar ieși mai bun decât în realitate, iar asta se numește data leakage.

# 3. Ce scor are un model care nu știe nimic?

Un model care nu știe nimic prezice mereu același lucru, în cazul nostru DummyClassifier spune despre toți clienții că rămân. Are un accuracy de 73.5%, care pare bun, dar e de fapt doar procentul celor care rămân. Recall-ul, precision-ul și F1 sunt 0, din cauza faptului că nu prinde niciun client care pleacă, toți cei 374 fiind ratați. Așadar accuracy-ul ne păcălește aici, iar modelele noastre trebuie să bată baseline-ul la recall și F1, nu la accuracy.

# 4. Care a ieșit mai bine și de ce credeți că s-a întâmplat asta?

Am antrenat regresie logistică, Random Forest și XGBoost, iar pe test au ieșit foarte apropiate, cu un F1 între 0.61 și 0.64. Random Forest a fost puțin mai bun, cu F1 de 0.636 și precision de 0.54. Cred că rezultatele sunt atât de apropiate din cauza faptului că decizia depinde mai ales de câteva coloane puternice, cum ar fi tipul de contract și vechimea, pe care le prinde bine și un model simplu. Random Forest iese puțin mai bine pentru că poate să prindă și combinații între coloane. GridSearchCV a ales aceiași parametri pe care îi aveam deja.

# 5. De ce ați ales metrica principală pe care ați ales-o?

Am ales F1 pe clasa celor care pleacă. Accuracy nu ne ajută aici, pentru că și baseline-ul are 73.5% fără să prindă niciun client care pleacă. Cel mai mult contează recall-ul, pentru că un client pierdut ne costă mai mult decât o reducere dată degeaba, dar doar recall-ul nu e suficient, un model care zice despre toți că pleacă ar avea recall 100%. Așadar F1 e o variantă bună pentru că ține cont și de recall și de precision.

# 6. Concluzie: ce model recomandați, cât de mult bate baseline-ul și o limitare a analizei

Recomand Random Forest, care din cei 374 de clienți care pleacă din setul de test prinde 290, adică un recall de 0.775, pe când baseline-ul nu prinde niciunul. F1 crește de la 0 la 0.636, iar la accuracy diferența e mică, de la 73.5% la 76.4%, ceea ce arată încă o dată că accuracy nu e metrica bună aici. Ca limitare, diferențele dintre modele sunt cam cât variază scorul între fold-uri, deci nu putem fi siguri că Random Forest e cu adevărat cel mai bun. De asemenea pragul de decizie a rămas 0.5 și nu l-am ajustat după cât costă de fapt greșelile.

# Lucrul cu AI

- **Ce i-am cerut agentului:** I-am cerut să-mi explice pașii proiectului și noțiunile pe care nu le înțelegeam, cum ar fi metricile spre exemplu, apoi să mă ajute pas cu pas la scrierea codului și verificarea acestuia.

- **Ce am schimbat din ce mi-a dat:** Am mai din cod în unele zone și am verificat pe date ideea  că unele coloane nu contează, `gender` chiar nu contează, dar `Partner` și `Dependents` da.

- **O greșeală de-a lui pe care am prins-o:** N-am prins nici o greșeală,
