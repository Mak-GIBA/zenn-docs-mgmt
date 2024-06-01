
# 無関係な下位問題を抽出する
プロジェクト固有のコードと汎用コードを分けることが重要。これによって，プロジェクトに固有の処理とそれ以外の部分に分離され，プロジェクト固有の処理に集中することができる。
## 低レベルの目標は関数化する
　コード上の無関係の下位問題を積極的に見つけて抽出することで，堅牢で読みやすいコードを作ることができる。コードから無関係の下位問題を抽出したい際，以下を自問すると良い。
1. 関数やコードブロックに対して「このコードの高レベル目標は何か？」
2. コードの各行に対して「高レベルの目標に直接的に効果があるのか？もしくは無関係の下位問題を解決しているのか？」
3. 「無関係の下位問題を解決しているコードが相当量ないか？」⇒相当量ある場合は，それらを抽出して別の関数にする

- Before:
```python
import json

def process_user_data(file_path):
    # データの読み込み
    with open(file_path, 'r') as file:
        data = json.load(file)
    
    # データの加工
    processed_data = []
    for user in data:
        processed_data.append({
            'name': user['first_name'] + ' ' + user['last_name'],
            'email': user['email']
        })
    
    # データの保存
    with open('processed_data.json', 'w') as file:
        json.dump(processed_data, file, indent=4)

process_user_data('user_data.json')
```
- After:
```python
import json

def load_data(file_path):
    """指定されたファイルからデータを読み込む関数"""
    with open(file_path, 'r') as file:
        return json.load(file)

def process_data(data):
    """データを加工する関数"""
    processed_data = []
    for user in data:
        processed_data.append({
            'name': f"{user['first_name']} {user['last_name']}",
            'email': user['email']
        })
    return processed_data

def save_data(data, file_path):
    """指定されたファイルにデータを保存する関数"""
    with open(file_path, 'w') as file:
        json.dump(data, file, indent=4)

def main(input_file, output_file):
    """メイン処理を行う関数"""
    data = load_data(input_file)
    processed_data = process_data(data)
    save_data(processed_data, output_file)

# メイン関数を呼び出して処理を実行
if __name__ == "__main__":
    main('user_data.json', 'processed_data.json')

```


## 汎用コードを積極的に作る
　良く使うような基本的な処理(例: 文字列の操作・ハッシュテーブルの使用・ファイルの読み書き)は，汎用コードとして複数のプロジェクトから再利用できるように独立させた方が良い。具体的には，処理を関数化してその関数群を特別なディレクトリ(例:`util/`)に格納しておく。ここで，作成した汎用コードは中身のことを考えずに使えるような形にしておく。
　また，抽出する処理はプロジェクトから完全に独立したものである方が良いが，完全に独立していなくてもそれはそれで問題ない。下位問題を抽出するだけでも十分に効果がある。

- Before:
```python
import json
import csv

def main():
    # データの読み込み
    with open('user_data.json', 'r') as file:
        data = json.load(file)

    # データの加工
    processed_data = []
    for user in data:
        processed_data.append({
            'name': user['first_name'] + ' ' + user['last_name'],
            'email': user['email']
        })

    # CSVへのデータの保存
    with open('processed_data.csv', 'w', newline='') as file:
        writer = csv.DictWriter(file, fieldnames=['name', 'email'])
        writer.writeheader()
        writer.writerows(processed_data)

if __name__ == "__main__":
    main()
```

- After:
```python
# ファイル構成
project/
│
├── main.py
└── utils/
    ├── file_operations.py
    └── data_processing.py
```
```python
# utils/file_operations.py
import json
import csv

def load_json(file_path):
    """指定されたファイルからJSONデータを読み込む関数"""
    with open(file_path, 'r') as file:
        return json.load(file)

def save_to_csv(data, file_path, fieldnames):
    """データをCSVファイルに保存する関数"""
    with open(file_path, 'w', newline='') as file:
        writer = csv.DictWriter(file, fieldnames=fieldnames)
        writer.writeheader()
        writer.writerows(data)

```
```python
# utils/data_processing.py
def process_user_data(data):
    """ユーザーデータを加工する関数"""
    processed_data = []
    for user in data:
        processed_data.append({
            'name': f"{user['first_name']} {user['last_name']}",
            'email': user['email']
        })
    return processed_data
```
```python
# main.py
from utils.file_operations import load_json, save_to_csv
from utils.data_processing import process_user_data

def main():
    # データの読み込み
    data = load_json('user_data.json')

    # データの加工
    processed_data = process_user_data(data)

    # CSVへのデータの保存
    save_to_csv(processed_data, 'processed_data.csv', fieldnames=['name', 'email'])

if __name__ == "__main__":
    main()
```

## 綺麗なインタフェースを持つライブラリを作る
引数が少ない・事前設定が少ない・面倒なことをしなくても使えるライブラリを積極的に作ることでコードの再利用性や可読性を向上させることが重要。つまり，インタフェースを整えて汚いコードを覆い隠す。
　インタフェースが綺麗でなくてもラッパー関数を作ることで対応することができる。
- 外部APIをラップする例(APIリクエストの詳細を隠すことで利用者が簡単に天気情報を取得できるようにする):
```python
import requests

def raw_api_request(url, headers, params):
    """
    外部APIへの生のリクエストを行う関数。
    
    Parameters:
    url (str): APIエンドポイント
    headers (dict): リクエストヘッダ
    params (dict): クエリパラメータ
    
    Returns:
    dict: APIレスポンス
    """
    response = requests.get(url, headers=headers, params=params)
    return response.json()

def get_weather(city):
    """
    天気情報を取得する簡素化された関数。
    
    Parameters:
    city (str): 都市名
    
    Returns:
    dict: 天気情報
    """
    url = "https://api.example.com/weather"
    headers = {"Authorization": "Bearer YOUR_API_KEY"}
    params = {"q": city}
    
    return raw_api_request(url, headers, params)

# 使用例
weather_info = get_weather("Tokyo")
print(weather_info)

```

# 一度に1つのタスクを実施する
