# package.xmlをフォーマット2に移行

ROS 1はすべてのpackage.xmlフォーマットバージョンをサポートしています。

## フォーマット1から2への移行
フォーマット1とフォーマット2は依存関係の指定方法が異なります。相違点の概要については、[REP-0140の互換性セクション](https://reps.openrobotics.org/rep-0140/#compatibility)をご覧ください。

package.xmlの先頭のタグ `<package>` が次のいずれかのようになっている場合は、フォーマット 1 が使用されているため、移行する必要があります。

```xml
<package>
```
```xml
<package format="1">
```

ROS 2では、package.xmlファイルは少なくともフォーマット2を使用する必要があります。

```xml
<package format="2">
```

## 依存関係の移行
`<run_depend>` タグは使用できなくなりました。

```xml
<run_depend>foo</run_depend>
```

ROS 2では、次のタグのいずれかまたは両方に置き換えます。

```xml
<build_export_depend>foo</build_export_depend>
<exec_depend>foo</exec_depend>
```

- パッケージに依存するパッケージがビルド時に依存関係が必要な場合は、`<build_export_depend>` タグを使用します。
- パッケージ内の何かを実行する際に依存関係が必要な場合は、`<exec_depend>` タグを使用します。
- 不明な場合は、両方のタグを使用してください。

## 依存関係を簡素化
同じ依存関係に対して以下のタグがある場合、

```xml
<build_depend>foo</build_depend>
<build_export_depend>foo</build_export_depend>
```

package.xmlファイルをより簡潔にする新しい `<depend>` タグに置き換えられます。

```xml
<depend>foo</depend>
```

## テスト依存関係（必要な場合）
フォーマット1において `<test_depend>` は「テストの実行時」に必要なパッケージのみを宣言するものでした。
フォーマット2以降では、これに加えて「テストのビルド時」に必要な依存関係も宣言できるようになっています。

旧フォーマットの制限により、テスト専用のパッケージ（例：testfoo）を以下のように `<build_depend>` として記述している場合があります。

```xml
<build_depend>testfoo</build_depend>
```

この記述を見つけた場合は、より適切な `<test_depend>` に変更してください。

```xml
<test_depend>testfoo</test_depend>
```

この修正により、テストを行わない一般ユーザーがパッケージをインストールする際、余計なテスト用ツールのダウンロードを避けられるようになります。
CMake を使用している場合は、テストの依存関係が `if(BUILD_TESTING)` ブロック内でのみ参照されていることを確認してください。

```cmake
if (BUILD_TESTING)
    find_package(testfoo REQUIRED)
endif()
```

## ドキュメント依存関係（必要な場合）
新しい `<doc_depend>` タグを使用して、パッケージのドキュメント構築に必要な依存関係を宣言します。例えば、C++ パッケージには次のような依存関係があるかもしれません。

```xml
<doc_depend>doxygen</doc_depend>
```

一方、Python パッケージには次のようなものがあるかもしれません。

```xml
<doc_depend>python3-sphinx</doc_depend>
```

詳細については、[ROS 2 パッケージのドキュメント化](https://docs.ros.org/en/humble/How-To-Guides/Documenting-a-ROS-2-Package.html)に関するガイドを参照してください。
