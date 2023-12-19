# Pengenalan MLOps dengan MLflow

## Sistem yang digunakan
* Sistem operasi Windows 10 64 bit.
* Sudah terinstal Anaconda, ikuti panduan [disini](https://docs.anaconda.com/anaconda/install/windows/) untuk instalasi.
* Sudah terinstal Visual Studio Code, ikuti panduan [disini](https://code.visualstudio.com/docs/setup/windows) untuk instalasi.
* Sudah terinstal Jupyter Notebook dalam Visual Studio Code, ikuti panduan [disini](https://towardsdatascience.com/installing-jupyter-notebook-support-in-visual-studio-code-91887d644c5d) untuk instalasi (gunakan mode incognito pada browser chrome).

## CARA INSTALL MLFLOW BACKEND SERVER
1. Buka Anaconda Prompt:

![alt text](https://github.com/randi-source/Workshop_Pengenalan_MLOps_dengan_MLflow/blob/main/Picture/anaconda_prompt.png)

2. Buat anaconda environment baru beserta beberapa dependensi library:
```console
conda create mlflow_env python==3.8.13 matplotlib scikit-learn
```
3. Aktifkan conda environment yang telah dibuat:
```console
conda activate mlflow_env
```
4. Install postgresql:
```console
conda install -y -c conda-forge postgresql
```

### SETTING POSGRESQL DATABASE

5. Buat PostgreSQL Database Cluster:
```console
initdb --pgdata=<path untuk PostgreSQL>
```
6. Start PostgreSQL Database:
```console
pg_ctl -D "<path PostgreSQL>" start
```
7. Start PostgreSQL di terminal dengan mengakses postgres database yang sudah ada dalam instalasi:
```console
psql --dbname postgres
```
#### Note:
Dalam PostgreSQL interactive terminal (terdapat tulisan "postgres=#")=

8. Buat database baru untuk mlflow, guna menyimpan registered model:
```psql
CREATE DATABASE mlflow_db;
```
9. Buat user dan password baru untuk mengautentikasi ketika hendak mengakses database:
```psql
CREATE USER mlflow_user WITH ENCRYPTED PASSWORD 'mlflow_user';
```
```psql
GRANT ALL PRIVILEGES ON DATABASE mlflow_db TO mlflow_user;
```
10. Cek User:
```psql
\du
```
11. Cek Database:
```psql
\list
```

12. Exit PostgreSQL interactive terminal:
```psql
\q
```

13. Database yang baru saja kita buat akan memiliki data terkait dengan model yang sudah di register. Konten tersebut dapat dilihat dengan perintah:
```psql
psql --dbname mlflow_db
```

14. Cek tabel yang telah dibuat MLflow:
```psql
\dt
```


## INSTALL MLFLOW

15. Install library MLflow:
```console
pip install mlflow
```

16. Install library psycopg2-binary, PostgreSQL adapter untuk Python:
```console
pip install psycopg2-binary
```

17. Membuat directory untuk menyimpan semua file yang dihasilkan:
```console
md mlflow\mlruns
```

18. Aktifkan PostgreSQL database:
```console
pg_ctl -D "D:/postgresql" -l logfile start
```

19. Menjalankan MLflow Tracking Server. Guna menjalankan, gunakan perintah mlflow server yang diikuti dengan PostgreSQL melalui --backend-store-uri dengan argumen <dialec><driver>://<username><password>@<host>:<post>/<database> , kemudian diikuti oleh perintah --defaut-artifact-root yang diikuti oleh path dimana kita membuat directory mlruns sebelumnya, contoh:
```console
mlflow server --backend-store-uri postgresql://mlflow_user:mlflow_user@localhost/mlflow_database --default-artifact-root file:/mlflow/mlruns -h localhost -p 8001
```
  
20. Akses MLflow pada browser, sesuaikan dengan host dan port yang telah didefinisikan pada perintah sebelumnya:

    http://localhost:8001

![alt text](https://github.com/randi-source/Workshop_Pengenalan_MLOps_dengan_MLflow/blob/main/Picture/mlflow_ui.jpeg)

#### Note:

* MLflow UI dan MLflow backend server akan berhenti bekerja jika user keluar dari anaconda prompt, maka jika hendak mengaktifkan kembali MLflow UI dan MLflow backend server, user perlu mengulangi langkah ke 18 dan 19. 
* File yang tersimpan dalam MLflow tidak akan hilang walau sudah keluar dari anaconda prompt

## PRAKTEK MLOPS MENGGUNAKAN MLFLOW
  
21. Buka Visual Studio Code:

22. Masuk ke dalam command pallete:
Ctrl+Shift+P

23. Copy paste Github URL kedalam command pallete untuk pull remote repository pengenalan_mlops_dengan_mlflow ke local repository:
  
    https://github.com/randi-source/Workshop_Pengenalan_MLOps_dengan_MLflow.git

24. Setelah pull repository berhasil, buka repository dan buka train.py dalam folder sklearn_elasticnet_wine. Contoh kasus diambil dari link berikut:
  
    https://github.com/mlflow/mlflow/tree/master/examples/sklearn_elasticnet_wine

25. Buka file customer_segmentation_kmeans.ipynb 

26. Pastikan kernel yang digunakan adalah mlflow_env:

![alt text](https://github.com/randi-source/Workshop_Pengenalan_MLOps_dengan_MLflow/blob/main/Picture/mlflow_env_kernel.png)

27. Jalankan Script

28. Buka MLflow ui pada browser: 
  
    http://localhost:8001


### Sumber: 

* Instalasi MLflow di Linux
  
  https://towardsdatascience.com/version-your-machine-learning-models-with-mlflow-9d6bbf8eb273

* Postgresql 
  
  https://www.postgresql.org/
  
* MLflow
  
  https://mlflow.org/
