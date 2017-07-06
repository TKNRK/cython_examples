# Cython 事始め

Cython を用いて Python のモジュールを作るのが目的．

## 作り方１

setup.py を使った普通な作り方． 作り方だけに注目しているので，サンプルコードは hello world で．

### .pyx を作成する

"hello world" スクリプトを例として作成．これを hello.pyx として保存する．

```
def say_hello_to(name):
  print("Hello %s !!" % name)
```

### setup.py を作成する

上記のモジュールをビルドするために setup.py スクリプトを作成する．  
このスクリプト作成には，モジュール作成のためのパッケージ distutils を用いる．

```
from distutils.core import setup
from distutils.extension import Extension
from Cython.Distutils import build_ext

ext_modules = [Extension("hello", ["hello.pyx"])]

setup(
  name = 'Hello world app',
  cmdclass = {'build_ext': build_ext},
  ext_modules = ext_modules
)
```

- Extension:
- setup()

### モジュールをビルドする

最後に，以下のコマンドを実行すると，hello.so とか色々とできて，モジュールとして hello.pyx のやつが使えるようになる．

```
$ python setup.py build_ext --inplace
```

### python で使ってみる

```
> import hello
> say_hello_to("Ikki")
Hello Ikki !!
```

### site packages に追加

いつもの

```
> import site
> site.getusersitepackages()
```

## 作り方２

　モジュールのビルドの際，外部の C ライブラリや特殊なビルド設定をしない場合，pyximport モジュールを用いて直接 import できます．  
　つまり，この場合はsetup.pyがいらない．

```
>>> import pyximport; pyximport.install()
>>> import hello
>>> hello.say_hello_to("Reona")
Hello Reona !!
```