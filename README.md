# jvm-memo


TSA Methodのデモ用レポジトリ。

# 動作確認環境

ubuntu 20.04

// TODO



## 実行方法

1. コンテナ起動

   ```shell
   docker-compose up -d --build
   ```

2. デモコンテナ内でasync-profilerを起動

   CPUプロファイリングの場合

   ```shell
   docker-compose exec demo bash
   /var/lib/async-profiler/profiler.sh -e cpu -d 60 -t -i 1ms -f "/app/output/demo_cpu.html" $(pgrep -nx java)
   ```

   wall-clockプロファイリングの場合

   ```shell
   docker-compose exec demo bash
   /var/lib/async-profiler/profiler.sh -e wall -d 60 -t -i 1ms -f "/app/output/demo_wall.html" $(pgrep -nx java)
   ```

   各オプション・引数について

   - `-e xxx` プロファイリングのタイプ指定
   - `-d 60` 60秒間解析
   - `-t` **スレッドごとにプロファイリング**
   - `-i 1ms` サンプリング間隔=1ms
   - `-f xxx` 出力ファイル名
   - `$(pgrep -nx java)` `java`を含むコマンドのプロセスIDを取得

3. 負荷をかける

   https://github.com/rakyll/hey を使用（別のコマンド可）


   ```shell
   hey -c 100 -n 10000 http://localhost:9090/demo
   ```
   //TODO

   ※ 別ホストから負荷を与えることを推奨


3. 出力ファイルをブラウザで開く

   ```shell
   open output/demo_cpu.html
   open output/demo_wall.html
   ```

