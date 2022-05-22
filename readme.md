## Rackとは
- アプリケーションサーバーとアプリケーションをつなぐもの。
- rackのおかげで、アプリがアプケーションサーバーに対して依存度が低くなる。
- 様々なアプリケーションサーバーに対応していることから、アプリケーションサーバーごとにアプリケーションとの通信方法を変える必要がなくなる。
## rackup
以下のコマンドで実行
```
$ rackup
```
app.rb
```rb
class App
  def call(env)
    pp env
    status = 200
    headers = { 'Content-Type' => 'text/plain' }
    body = ['sample']
    [status, headers, body]
  end
end
```

config.ru
```ruby
# アプリケーションを作成するためのファイル
# rackライブラリを読み込むことでrunメソッドが使用可
# rackアプリケーションのAppのインスタンスをrunメソッドで実行
require 'rack'
require_relative 'app'

run App.new

```
[![Image from Gyazo](https://i.gyazo.com/131ce1260871b9665e807115d37fc84f.png)](https://gyazo.com/131ce1260871b9665e807115d37fc84f)
[![Image from Gyazo](https://i.gyazo.com/de3952d56235ba3912316ae1ed1cbcb0.png)](https://gyazo.com/de3952d56235ba3912316ae1ed1cbcb0)
[![Image from Gyazo](https://i.gyazo.com/60ce8096266a29a96c8091a5ad9d82d9.png)](https://gyazo.com/60ce8096266a29a96c8091a5ad9d82d9)

## callメソッドの中身を確認
callメソッドでは、httpリクエストを引数として受け取り、戻り値をhttpレスポンスとして返す。
```
{"rack.version"=>[1, 6],
 "rack.errors"=>
  #<Rack::Lint::ErrorWrapper:0x00007f989111d150 @error=#<IO:<STDERR>>>,
 "rack.multithread"=>true,
 "rack.multiprocess"=>false,
 "rack.run_once"=>false,
 "rack.url_scheme"=>"http",
 "SCRIPT_NAME"=>"",

 --- 省略 ---

     @options=
      {:environment=>"development",
       :pid=>nil,
       :Port=>9292,
       :Host=>"localhost",
       :AccessLog=>[],
       :config=>
        "/Users/makoto.k/program/learning/rails_practice/rack_practice/config.ru",
       :binds=>["tcp://localhost:9292"],
       :app=>
        #<Rack::ContentLength:0x00007f98910fc5e0
         @app=
          #<Rack::CommonLogger:0x00007f98910fc658
           @app=
            #<Rack::ShowExceptions:0x00007f9894810248
             @app=
              #<Rack::Lint:0x00007f9894811300
               @app=
                #<Rack::TempfileReaper:0x00007f98948118f0
                 @app=#<App:0x00007f9891087a38>>,
               @content_length=nil>>,
           @logger=#<IO:<STDERR>>>>},
     @plugins=[]>>,
 "rack.tempfiles"=>[]}
```

## rackミドルウェア
- アプリケーションとアプリケーションサーバの間に処理を追加するrackに備わっているミドルウェアのこと。
- Rack::Runtime
  - リクエストを受け取ってからレスポンスを返すまでの時間をレスポンスヘッダーに追加する
[![Image from Gyazo](https://i.gyazo.com/b50e60b5d843721fd9ce98ee1069faba.png)](https://gyazo.com/b50e60b5d843721fd9ce98ee1069faba)
