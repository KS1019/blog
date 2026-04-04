---
title: vImage.PixelBuffer.init(width:height:pixelFormat:)ではまった
description: ""
date: 2026-03-20T08:07:27.417Z
preview: ""
draft: true
tags: []
categories: []
---

vImage.PixelBuffer.init(width:height:pixelFormat:)というイニシャライザーを使ってリサイズ後の画像データを収めるバッファー用の変数を用意しているコードがありました。

そのコードで一定確率でクラッシュが起こっていたのですが、原因としては`width`か`height`に0を入れてしまっていたことでした。（少数を含むサイズを`Int`型にキャストした時に意図せず、0になってしまっていた。）おそらくこのイニシャライザ内の`precondition`でクラッシュするようになっているが、特にその旨がエラーなどに出ないので気づくまで詰まりました。