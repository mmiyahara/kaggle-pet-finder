# [kaggle-pet-finder](https://www.kaggle.com/c/petfinder-pawpularity-score)

![schedule](assets/diagram.png)

## Overview

- Pet の人気度を推定する
- 結果の Submit は 1 日 2 回まで

## Diagram

- Init

  ```
  docker pull minlag/mermaid-cli
  ```

- Edit

  ```
  vi assets/diagram.mmd
  ```

- Build

  ```
  rm -f assets/diagram.png
  docker run -it -v $(pwd)/assets:/data minlag/mermaid-cli -i /data/diagram.mmd -o /data/diagram.png
  ```

## Docs

| Title                                                                                            | Read       | Comment |
| ------------------------------------------------------------------------------------------------ | ---------- | ------- |
| [つくりながら学ぶ! PyTorch による発展ディープラーニング](https://www.amazon.co.jp/dp/4839970254) | 2021/10/xx |         |
|                                                                                                  | 2021/10/xx |         |
|                                                                                                  | 2021/10/xx |         |

## Diary

### 2021/10/03

- Google Colab で `submission.csv` まで作って出したら `Submission Scoring Error` に

  > How will winners be determined?
  > In some code competitions, winners will be determined by re-running selected submissions’ associated Notebooks on a private test set.
  >
  > In such competitions, you will create your models in Notebooks and make submissions based on the test set provided on the Data page. You will make submissions from your Notebook using the above steps and select submissions for final judging from the “My Submissions” page, in the same manner as a regular competition.
  >
  > Following the competition deadline, your code will be rerun by Kaggle on a private test set that is not provided to you. Your model's score against this private test set will determine your ranking on the private leaderboard and final standing in the competition.
  > https://www.kaggle.com/docs/competitions#notebooks-only-FAQ

  モデルの事前学習だけ Google Colab で、予測を Kaggle Notebook ですれば問題なさそう。  
  画像データからの特徴量作成も Kaggle Notebook でする必要がありそう。

### 2021/10/04

- とりあえず LightGBM で回してみた([2021/10/03 Pet Finder - Light GBM Baseline](https://www.kaggle.com/mstkmyhr/2021-10-03-pet-finder-light-gbm-baseline))(score: , time: )

- PyTorch の使い方、ディープラーニングでの標準的なワークフローについて知るため、[つくりながら学ぶ! PyTorch による発展ディープラーニング](https://www.amazon.co.jp/dp/4839970254) を読み始めた。

  - PyTorch 日本語チュートリアル翻訳者が書いた本で、チュートリアルの訳注がわかりやすかったので購入

- [211004_01_image_classification.ipynb - Colaboratory](https://colab.research.google.com/drive/1xzfcu9Oe3pizjKA7lWNJqP1kYVdAIt3w#scrollTo=XzK_GlZyhHff)

- [zipfile --- ZIP アーカイブの処理 — Python 3.9.4 ドキュメント](https://docs.python.org/ja/3/library/zipfile.html#zipfile-objects)

  - Python 組み込みモジュール。JavaScript には外部ライブラリしかないはず。組み込みで容易されてるのは便利。

  ```python
  path = '/path/to/zipfile.zip'
  dist_dir = '/path/to/distdir'
  zip = zipfile.ZipFile(path)
  zip.extractall(dist_dir)
  zip.close()
  ```

- [Pillow — Pillow (PIL Fork) 8.3.2 documentation](https://pillow.readthedocs.io/en/stable/)

  - 画像処理用のライブラリ。OpenCV ほど多機能ではないが、拡大、回転などの基本機能があって使いやすい。

  ```python
  # TBD
  ```

- [pytorch/vision: Datasets, Transforms and Models specific to Computer Vision](https://github.com/pytorch/vision)

  - ポピュラーなデータセット（ImageNet 等）や学習済みモデル、画像処理用の関数などが含まれているモジュール。

  ```python
  # 学習済みモデルの読み込み
  from torchvision import models
  net = models.vgg16(pretrained=True)
  print(net)
  # VGG(
  #   (features): Sequential(
  #     (0): Conv2d(3, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
  #     ...
  #     (6): Linear(in_features=4096, out_features=1000, bias=True)
  #   )
  # )
  ```

- [VGG16 - Visual Geometry Group - University of Oxford](https://www.robots.ox.ac.uk/~vgg/research/very_deep/)
  - [ ] [vision/vgg.py at main · pytorch/vision](https://github.com/pytorch/vision/blob/main/torchvision/models/vgg.py)
  - [ ] [VGG16 について調べてみた | bassbone's blog](https://blog.bassbone.tokyo/archives/652)
  - [ ] [[1409.1556] Very Deep Convolutional Networks for Large-Scale Image Recognition](https://arxiv.org/abs/1409.1556)

### 2021/10/05

- [torchvision.transforms — Torchvision 0.10.0 documentation](https://pytorch.org/vision/stable/transforms.html)

  - PyTorch で用意されている画像変換のモジュール
  - [Illustration of transforms — Torchvision 0.10.0 documentation](https://pytorch.org/vision/stable/auto_examples/plot_transforms.html)
  - [Tensor transforms and JIT — Torchvision 0.10.0 documentation](https://pytorch.org/vision/stable/auto_examples/plot_scripted_tensor_transforms.html#sphx-glr-auto-examples-plot-scripted-tensor-transforms-py)

- [NumPy 配列 ndarray を任意の最小値・最大値に収める clip | note.nkmk.me](https://note.nkmk.me/python-numpy-clip/)

  - 引数に最小値、最大値を取り、範囲外の値を最小値、最大値に置き換える
  - 0.0 - 1.0、0 - 255 に置き換えるときによく使う

  ```python
  a = np.arange(10)
  print(a)
  # [0 1 2 3 4 5 6 7 8 9]

  print(np.clip(a, 2, 8))
  # [2 2 2 3 4 5 6 7 8 8]
  ```
