# DataPipeline

ETL 基礎概念

E (Extract)：從資料來源取得資料（如 API、資料庫、網頁等）

T (Transform)：資料清洗、轉換、格式化（使用 Pandas 等工具）

L (Load)：將資料載入目標資料庫（如 MySQL、MongoDB、BigQuery）

Pandas 操作筆記

1. 基礎結構
   
DataFrame: 二維表格結構，欄位名稱＋列索引

建立 DataFrame：

import pandas as pd

df = pd.DataFrame(data)

2. 空值處理

NaN 與 Python 的 None 不同

判斷：df.isna() / df.isnull()

丟棄空值列：df.dropna()

補值：

前向填補：df.ffill()

後向填補：df.bfill()

3. 時間格式處理
   
轉換時間欄位：

df['time'] = df['time'].apply(pd.to_datetime)

錯誤格式會變成 NaT

5. DataFrame 操作
   
合併資料（類似 SQL JOIN）：

pd.merge(df1, df2, on='key', how='left')

欄位轉為列（melt）：

df_melt = pd.melt(df, id_vars=['id'], var_name='variable', value_name='value')

SQL 語法操作 Pandas：

import pandasql as ps

ps.sqldf("SELECT * FROM df WHERE col > 5", locals())

7. 拷貝與修改
   
複製 DataFrame：df.copy()

避免 chained assignment 錯誤警告

資料庫連線操作

# MySQL（pymysql）

import pymysql

conn = pymysql.connect(host, user, password, db, charset='utf8')

cursor = conn.cursor()

cursor.executemany("INSERT INTO table VALUES (%s, %s)", data)

conn.commit()

# MongoDB（pymongo）

from pymongo import MongoClient

client = MongoClient('mongodb://localhost:27017/')

db = client['mydb']

collection = db['mycollection']

collection.insert_many(data)

# Pandas 讀取 SQL 資料

import pandas as pd

df = pd.read_sql("SELECT * FROM table", conn)


# 網頁爬蟲工具

# requests + BeautifulSoup

import requests

from bs4 import BeautifulSoup

res = requests.get('https://example.com')

soup = BeautifulSoup(res.text, 'html.parser')

titles = soup.find_all('h2')

# selenium（動態網頁）

from selenium import webdriver

driver = webdriver.Chrome()

driver.get("https://maps.google.com")

page = driver.page_source

# 專案管理與 Poetry + DevContainer

Poetry

建立專案：poetry new my_project

安裝套件：poetry add pandas pymysql ...

使用 pyproject.toml 管理環境

DevContainer

.devcontainer/devcontainer.json 結合 VSCode Remote Development

配合 Dockerfile 自動建構虛擬環境

# Airflow 流程排程工具

安裝與啟動

docker-compose up airflow-init

docker-compose up

# DAG（Directed Acyclic Graph）

每個 DAG 是一組排程工作流程

基本結構：

from airflow import DAG

from airflow.operators.python import PythonOperator

from datetime import datetime

def my_task():
    print("Hello Airflow")

with DAG(dag_id="example", start_date=datetime(2024, 1, 1), schedule_interval="@daily") as dag:
    task = PythonOperator(task_id="task1", python_callable=my_task)

# 常見功能

失敗重試：retries=3, retry_delay=timedelta(minutes=5)

通知機制：email 或 Slack webhook

Web UI 控制台：localhost:8080

# Prefect（現代化替代 Airflow）

基本使用

from prefect import flow, task

@task

def say_hello():
    print("Hello Prefect")

@flow
def my_flow():
    say_hello()

my_flow()

支援雲端控制台與本地執行

UI 簡潔易用，支援流量監控與排程

# Flask Web 應用框架

快速啟動

from flask import Flask, request

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello, Flask!"

@app.route('/api', methods=['POST'])
def api():
    data = request.json
    return {'result': data}

if __name__ == '__main__':
    app.run(debug=True)

適合建立 API、Dashboard、小型 Web UI

可與資料庫、Pandas、ETL 流程結合
