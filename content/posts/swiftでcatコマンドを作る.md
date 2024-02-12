---
title: SwiftでCatコマンドを作る
description: ""
date: 2023-02-16T05:52:08.451Z
draft: false
tags:
  - cli
  - swift
  - command
categories: ""
slug: build-your-own-cat-in-swift
keywords:
  - cat
  - swift
  - cli
aliases: 
    - /test-alias1
    - /test-alias2
---

## catコマンド

catコマンドとは、与えられたファイルパスの中身を標準出力に書き込むコマンドです。今回はそれをSwiftで再現してみたいと思います。

```shell
     The cat utility reads files sequentially, writing them to the standard
     output.  The file operands are processed in command-line order.  If file
     is a single dash (‘-’) or absent, cat reads from the standard input.  If
     file is a UNIX domain socket, cat connects to it and then reads it until
     EOF.  This complements the UNIX domain binding capability available in
     inetd(8).
```

今回は、一つのファイルを読んで出力するところまでを再現します。

## Swiftからシステムコールを呼ぶ

Swiftから`read`や`write`などのシステムコールを呼ぶために、今回は[apple/swift-system: Low-level system calls and types for Swift](https://github.com/apple/swift-system)というApple製のSwift Packageを利用することにしました。

```swift
import SystemPackage
```

としてインポートすることができます。

## コマンドを読み込む

Swiftでは以下のようにすることでターミナルから引数を読み込むことができます。

```swift
let argv = CommandLine.arguments
let argc = CommandLine.arguments.count
```

## 実行とテスト

```shell
swift run cat [ file ]
```

今回は`cat`という`executableTarget`を作成しているので上記のようにして実行することができます。またテストする際は以下のようにして行いました。

```shell
diff -a <(cat Sources/cat/cat.swift) <(swift run cat Sources/cat/cat.swift)
```

差分がなければ標準出力には何も表示されずに正常終了します。

## Swiftコード全体

```swift
import SystemPackage

@main
struct Cat {
    public static func main() throws {
        let argv = CommandLine.arguments
        let argc = CommandLine.arguments.count

        let buf = UnsafeMutableRawBufferPointer.allocate(byteCount: 4096, alignment: 0)
        var fd: FileDescriptor = FileDescriptor.standardInput

        if argc != 2 {
            fatalError("Usage: \(argv[0]) [ file ]")
        } else {
            do {
                fd = try FileDescriptor.open(argv[1], .readOnly)
            } catch {
                fatalError("cannot open \(argv[1]): \(error)")
            }

            do {
                let bytes = try fd.read(into: buf)
                try fd.close()
                let _ = try FileDescriptor.standardOutput.writeAll(buf[..<bytes])
                buf.deallocate()
            } catch {
                fatalError("Error: \(error)")
            }
        }
    }
}
```

## 参考

- Catの実装は以下の動画を参考にしました。

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/P6w_5gJw3IY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
